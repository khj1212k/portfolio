[한국어](./newsletter-langgraph.md) | [English](./newsletter-langgraph.en.md)

# LangGraph Newsletter Generation — One-pass drafts mixed separate events and still hallucinated

> Boostcamp AI Tech 8th final project | Leader of a five-person team, then solo follow-up | Jan to Feb 2026

The team built a service that groups articles about the same event, has an LLM write them up as one newsletter, and ranks the newsletters for each user.

As team leader I led the architecture and module interface design and owned the LLM generation pipeline.

I built a generation flow capped at three attempts, in which a draft that still fails evaluation is not saved, and after the team project I directed an AI coding agent on a fork and added a check that compares numbers and quotes with the source articles.

---

## What I Did in the Team Phase

```mermaid
flowchart TD
    A["RSS collection and embedding 7 times a day"] --> B["HDBSCAN clustering once a day"]
    B --> C["Second-stage split"]
    C --> D{"LLM checks whether it is one event"}
    D -->|"mixed"| E["Remove outliers or keep largest sub-group"]
    E -->|"judge again"| D
    E -->|"cannot be cleaned up"| S["Skip"]
    D -->|"pass"| G["Generate newsletter"]
    G --> F["Source check"]
    F -->|"number or quote not in sources"| G
    F -->|"third attempt fails"| X["Not saved"]
    F -->|"pass"| J{"Evaluator node"}
    J -->|"fail, comments accumulate"| G
    J -->|"third attempt fails"| X
    J -->|"pass"| T["Tone conversion"]
    T --> R["Check numbers and dates after conversion"]
    R --> P[("Save to PostgreSQL pgvector")]
    classDef followup stroke-dasharray: 5 4
    class F,R followup
```

Solid boxes are the flow designed in the team phase; the two dashed boxes are checks added in the solo follow-up.

### 1. Separate generation and evaluation — Hallucination and omissions needed a structural check

**Problem**: Prompt tuning alone could not reliably prevent hallucination or missing key facts.

**Solution**: Separated the writing node from the judging node in LangGraph. In the evaluator node an LLM (HyperCLOVA X) scores the draft from 0 to 10 on missing key facts, structure and hallucination, and only 5 or above passes. On a fail, the evaluator's comments accumulate across attempts and go into the next generation.

**Result**: To bound cost and latency, capped generation at three attempts per article cluster; if the third draft also fails, nothing is saved. Stored each saved newsletter with its per-attempt scores and comments.

### 2. Two-stage clustering — First-pass clusters mixed separate events

**Problem**: The first-pass clusters, built by grouping BGE-M3 embeddings with HDBSCAN, mixed separate events, so one newsletter covered several topics.

**Solution**: Tried splitting every cluster of four or more articles in two, then decided whether to keep the split from the Silhouette Score, the overlap of named entities in titles and the share of duplicate titles. In the first LangGraph judging node, an LLM checks whether the cluster is one event; if it is mixed, the outlying articles are removed or only the largest sub-group is kept, and the cluster is judged again.

**Result**: A flow that skips any cluster that cannot be cleaned up into one event or shrinks to fewer than three articles.

### 3. Batch schedule and interfaces — Clustering at every collection split one event into pieces

**Problem**: Articles about one event come out over the course of a day, so clustering at every collection splits one event across several newsletters. Each module also expected a different LLM response format, and parse failures and rate-limit errors kept recurring.

**Solution**: Split the Airflow batches in two: collection, body extraction and embedding run seven times a day, clustering and generation once. Agreed data formats, APIs and database storage rules with the team before implementation. Moved retries and parsing into a single unified LLM client.

**Result**: Two Airflow DAGs, one collecting seven times a day and one clustering and generating once a day. Retry upper bounds were added in item 4.

---

## What I Did in the Solo Follow-up

This is the work after the final team commit `ef7c176` (2026-02-10), ongoing on the fork since September 2026. An AI coding agent, Claude Code, did the implementation and researched the design options; I set the direction and the verification criteria.

### 4. Source check — Adding source comparison of numbers and quotes to the LLM judge

**Problem**: The team-phase evaluator judged from the source titles. The next step was to check numbers and quotes against the full article text, and to apply the same bar to the tone-converted version and to every retry path.

**Solution**: Added a rule-based check that compares the numbers and quotes in a draft with the full text of the cluster's articles and forces a rewrite when one is not in the sources. Chose it over an LLM-judge-only option and a natural language inference (NLI) model because it costs nothing, is reproducible and can point to the wrong value. Also added a check of numbers and dates after tone conversion and an upper bound on the three retry paths.

**Result**: Caught all 500 synthetic cases where one number was scaled by 1.1, with 0 false positives on 494 numbers rewritten only in an equivalent notation. A test that runs the whole graph with a fake LLM confirms that when a number absent from the sources keeps appearing, three generations end with nothing saved.

---

## Results

| Item | Result | Conditions |
| --- | --- | --- |
| Generation flow, team phase | At most three attempts; a failing draft is not saved | Graph branches and settings at the final team commit |
| Batch schedule, team phase | Collection 7 times a day, clustering and generation once | Two Airflow DAGs at the final team commit |
| Source check, follow-up | 500 of 500 altered numbers caught; 0 of 494 false positives | Synthetic perturbations of 401 archived team newsletters |
| Retry bounds, follow-up | Upper bounds added to three retry paths | Tests check that all three stop at their bound |

---

## More

<details>
<summary><b>My scope and the team's</b></summary>

| Area | Owner |
| --- | --- |
| Overall architecture, data flow, module interfaces | Led by me; formats and storage rules agreed by the team |
| LangGraph generation and evaluation flow | Me |
| RSS collection and cleaning, embedding and clustering, newsletter writing and prompts, unified LLM client | Led by me (listed jointly in the team README) |
| Airflow batches, ERD design, planning | Shared between me and teammates |
| Recommendation model, FastAPI backend and database, frontend, synthetic dataset | Teammates |

The service as a whole, including recommendation and the UI, is a team result. In the anonymous peer review, all four teammates named clear communication as a strength and three named leadership. Everything up to the final team commit is team work and the 832 commits after it are my solo follow-up, so the boundary is in the Git history.

</details>

<details>
<summary><b>Where AI coding agents were used</b></summary>

- What the agent did: the code, tests and documents in the 832 commits after the final team commit; researching and proposing design options; the July 2026 code review
- What I decided: the direction of putting the AI part first, continuing on the fork, having design decisions recorded as ADRs, and disclosing the use of an agent
- How I checked: CI tests on every push, a test that runs the whole graph with a fake LLM, synthetic perturbation measurement
- Technology added in the follow-up: an OpenAI-compatible LLM adapter (Gemini), Docker Compose, GitHub Actions
- Items 1 to 3 are based on code committed to the team repository under my account. In January 2026 I also built a separate prototype of the same task with Codex to compare collection approaches

</details>

<details>
<summary><b>What you can check</b></summary>

- [Fork repository](https://github.com/khj1212k/pro-recsys-finalproject-recsys-01): code, tests/, docs/adr/, CI tests on every push
- [workflow/graph.py](https://github.com/khj1212k/pro-recsys-finalproject-recsys-01/blob/ef7c176c4f795aad0d70e1c63ebd026065f4cd1e/ai_workspace/workflow/graph.py): team-phase generation, evaluation and regeneration branches
- [workflow/nodes.py](https://github.com/khj1212k/pro-recsys-finalproject-recsys-01/blob/ef7c176c4f795aad0d70e1c63ebd026065f4cd1e/ai_workspace/workflow/nodes.py), [workflow/evaluators.py](https://github.com/khj1212k/pro-recsys-finalproject-recsys-01/blob/ef7c176c4f795aad0d70e1c63ebd026065f4cd1e/ai_workspace/workflow/evaluators.py): accumulated comments, judging criteria
- [core/clustering/split_v2.py](https://github.com/khj1212k/pro-recsys-finalproject-recsys-01/blob/ef7c176c4f795aad0d70e1c63ebd026065f4cd1e/ai_workspace/core/clustering/split_v2.py): second-stage split
- [ADR 0010](https://github.com/khj1212k/pro-recsys-finalproject-recsys-01/blob/a797aba9f1a4cbefb20cab4f68b82c67e70bdc72/docs/adr/0010-faithfulness-gate-and-judge-v2.md): options and evidence for the source check
- [Graph-level tests](https://github.com/khj1212k/pro-recsys-finalproject-recsys-01/blob/a797aba9f1a4cbefb20cab4f68b82c67e70bdc72/tests/test_workflow_faithfulness_gates.py): the whole LangGraph flow run with a fake LLM
- [ADR 0009](https://github.com/khj1212k/pro-recsys-finalproject-recsys-01/blob/a797aba9f1a4cbefb20cab4f68b82c67e70bdc72/docs/adr/0009-llm-eval-protocol-and-preregistered-decision-rule.md): pre-registered LLM evaluation rule
- [July 2026 review log](https://github.com/khj1212k/pro-recsys-finalproject-recsys-01/blob/a797aba9f1a4cbefb20cab4f68b82c67e70bdc72/docs/fix-log-2026-07.md), [commit `e962deb`](https://github.com/khj1212k/pro-recsys-finalproject-recsys-01/commit/e962deb18695d648c83679bb36b835688e7a3f93): retry bound fixes
- [Commit history](https://github.com/khj1212k/pro-recsys-finalproject-recsys-01/commits/a797aba9f1a4cbefb20cab4f68b82c67e70bdc72): 832 commits after the final team commit, 23 design decision records, 63 merged PRs (as of 2026-11-12). These counts cover the whole repository, including the recommendation work
- [Original team repository](https://github.com/boostcampaitech8/pro-recsys-finalproject-recsys-01)

Team-phase links are pinned to the final team commit `ef7c176`, follow-up links to fork commit `a797aba` (2026-11-12).

</details>

<details>
<summary><b>Team</b></summary>

| Name | Areas | GitHub |
| --- | --- | --- |
| 김형준 (Heung Jun Kim) | LangGraph, clustering, planning, newsletter writing, crawling, ERD design, LLM, prompt engineering, Airflow | [@khj1212k](https://github.com/khj1212k) |
| 박소정 | Back end, Airflow, database setup, ERD design, planning | [@sojeong0202](https://github.com/sojeong0202) |
| 성승우 | Recommendation model, planning | [@sssseungu](https://github.com/sssseungu) |
| 이선진 | Front end, back end, planning, synthetic dataset generation, database setup, ERD design, prototype implementation | [@Seonjin-13](https://github.com/Seonjin-13) |
| 이효창 | Planning, crawling, clustering, newsletter writing, LLM, prompt engineering | [@hochang2](https://github.com/hochang2) |

</details>

---
