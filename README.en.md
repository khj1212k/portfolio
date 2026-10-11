[한국어](./README.md) | [English](./README.en.md)

# Heung Jun Kim (김형준)

New-graduate AI engineer. I built an LLM translation and review flow (Trip.com internship) and a LangGraph newsletter generation and evaluation pipeline (leader of a 5-person team), and fine-tuned a 7B vision-language model (graduation thesis, two first-author conference papers). I set the criteria first, then check what LLMs and AI coding agents produce against them.

Wherever implementation was delegated to AI coding agents, each case study separates what the agents did from what I decided and checked ([section below](#how-i-use-ai-coding-agents)).

Verifiable outside this repository

- [Newsletter fork](https://github.com/khj1212k/pro-recsys-finalproject-recsys-01): code, tests/ and docs/adr/, CI passing
- [DACON Phase 2 leaderboard](https://dacon.io/competitions/official/236743/leaderboard): 17th row (1,184.97753 points)
- [Training script fork](https://github.com/khj1212k/lmms-finetune): 5 commits from 2025-04-29 to 05-08

Contact: [LinkedIn](https://www.linkedin.com/in/heung-jun-kim-46652031a) | [GitHub](https://github.com/khj1212k)

## At a glance

Headline figures and the conditions under which they were measured.

- **Korean coverage of 9,755 attractions from 72.3% to 100%**: Trip.com internship, attractions in China rated 4.0 or higher, Nov 2024 vs. Feb 2025
- **LangGraph newsletter generation and evaluation pipeline**: leader of a 5-person team. After the team project, 463 commits on the fork, 19 design decision records (ADRs), CI tests that run on every push and pass on the latest commit as of 2026-10-06 (implemented by an AI coding agent; the direction and the verification criteria were mine)
- **My exploration track from 783.78 to 983.26 points**: LG Aimers 9th hackathon. After the top local-validation combination dropped to 415 points on submission, I rebuilt validation to match real submission conditions and submitted only versions that passed; these are the leaderboard scores received (experiments implemented by AI coding agents; submission thresholds and stop decisions were mine). Team result: 17th of 1,030 teams, one of 34 teams in the final
- **97.05% same-person accuracy**: graduation thesis, Market-1501, full fine-tuning of a 7B vision-language model. The same research produced two first-author conference papers (ITAM-31, ChineseCSCW 2025)

| Project | Problem | What I did | Where to check |
| --- | --- | --- | --- |
| [Trip.com LLM translation pipeline](./projects/tripcom-llm-translation.en.md)<br>Internship, Nov 2024 to Feb 2025 | Only 72.3% of attractions rated 4.0 or higher had Korean information, and translation was done by hand, one entry at a time | Proposed LLM translation in an internal document, designed the review rule (check first the fields where an error harms a traveler) and the prompt template, compared five LLMs on identical inputs, wrote the merge script | Prose and a redrawn flow diagram. Company material, so the code and the actual prompts are not public |
| [LangGraph newsletter generation](./projects/newsletter-langgraph.en.md)<br>Leader of a 5-person team, Jan to Feb 2026, then solo follow-up | Asking an LLM to write from many articles at once mixed separate events and could not reliably prevent hallucination or missing facts | Split generation and evaluation into LangGraph nodes, added a second clustering stage, designed the batch schedule. After the team project, added a source-comparison check on a fork (implemented by an AI coding agent) | [Code and tests](https://github.com/khj1212k/pro-recsys-finalproject-recsys-01), CI passing; [ADR 0010](https://github.com/khj1212k/pro-recsys-finalproject-recsys-01/blob/1a42b46c6efa93431e1dd0eb5fff0083ce14d914/docs/adr/0010-faithfulness-gate-and-judge-v2.md); [graph-level tests](https://github.com/khj1212k/pro-recsys-finalproject-recsys-01/blob/1a42b46c6efa93431e1dd0eb5fff0083ce14d914/tests/test_workflow_faithfulness_gates.py) |
| [LG Aimers 9th hackathon](./projects/lg-aimers-agent-validation.en.md)<br>5-person team, Aug to Sep 2026 | The top local-validation combination dropped to 415 points on submission | Rebuilt validation to match real submission conditions and submitted only versions that passed (implemented by AI coding agents) | [DACON leaderboard](https://dacon.io/competitions/official/236743/leaderboard), 17th row. Code is private under competition rules |
| [Person re-identification with a vision-language model](./projects/vlm-person-reid-thesis.en.md)<br>Individual research, Mar to May 2025 | Can a vision-language model do person re-identification, and which fine-tuning method fits a limited GPU budget | Compared full fine-tuning with three PEFT methods under matched training time on a 7B model, designed a two-stage transfer | [Training script fork](https://github.com/khj1212k/lmms-finetune), 5 commits. The two papers are given as bibliographic details |

Other work

- VisuAI (Dec 2022 to Jan 2023, 3-person team): a walking-assistance web app that detects obstacles with the phone camera and announces them by voice. We fine-tuned YOLOv5-nano and converted it to TensorFlow.js to run in the browser.

## Reading order

- 1 minute: the table above
- 5 minutes: to see code, item 4 of the [newsletter case](./projects/newsletter-langgraph.en.md) and, on the fork, [ADR 0010](https://github.com/khj1212k/pro-recsys-finalproject-recsys-01/blob/1a42b46c6efa93431e1dd0eb5fff0083ce14d914/docs/adr/0010-faithfulness-gate-and-judge-v2.md) and the [graph-level tests](https://github.com/khj1212k/pro-recsys-finalproject-recsys-01/blob/1a42b46c6efa93431e1dd0eb5fff0083ce14d914/tests/test_workflow_faithfulness_gates.py). To see a result applied on the job, the [Trip.com case](./projects/tripcom-llm-translation.en.md)
- More: the [LG Aimers case](./projects/lg-aimers-agent-validation.en.md) (running AI coding agents and checking their output) and the [thesis case](./projects/vlm-person-reid-thesis.en.md)

Where to start, by role.

- AI agent roles: items 1, 2 and 4 of the [newsletter case](./projects/newsletter-langgraph.en.md) (LangGraph judging nodes, retry bounds, source-comparison check), then the [LG Aimers case](./projects/lg-aimers-agent-validation.en.md) (running and checking AI coding agents)
- Roles working on LLM output quality: items 1 and 4 of the [newsletter case](./projects/newsletter-langgraph.en.md), and the [Trip.com case](./projects/tripcom-llm-translation.en.md)
- AI transformation (AX) and data roles: the [Trip.com case](./projects/tripcom-llm-translation.en.md), item 3 of the [newsletter case](./projects/newsletter-langgraph.en.md), and the [LG Aimers case](./projects/lg-aimers-agent-validation.en.md) (tabular data and validation)
- Model training roles: the [thesis case](./projects/vlm-person-reid-thesis.en.md), and the GBDT models and features in the [LG Aimers case](./projects/lg-aimers-agent-validation.en.md)

Every case study follows the same order: problem, what I did and why, results and how they were checked, my scope and the team's (the solo thesis case lists its papers instead), limits, what you can check. The newsletter and LG Aimers cases add a section on where AI coding agents were used, placed before the limits.

This repository holds case studies, not code. Code is linked from the "What you can check" section of each case. The code you can open is the newsletter fork and the thesis training-script fork; the Trip.com case is company material and the LG Aimers case is bound by competition rules, so both are written as prose and redrawn diagrams only.

## Papers

1. **Kim, H. J.**, et al. (2025). "A Study of Person Re-identification Based on Vision-Language Multimodal Large Language Model." 31st International Conference on IT Applications and Management (ITAM-31), proceedings pp. 252-259.
2. **Kim, H. J.**, et al. (2025). "A Cost-Effective Training Method for Large Models in Pedestrian Search Task." 20th CCF Conference on Computer Supported Cooperative Work and Social Computing (ChineseCSCW 2025).

Both papers report the [graduation thesis research](./projects/vlm-person-reid-thesis.en.md).

## How I work

- **Criteria first.** At Trip.com thousands of entries could not all be reviewed, so I decided to review first the fields where an error harms a traveler. In the newsletter project, a draft that does not pass evaluation within three attempts is not saved.
- **Results are checked under real conditions.** In LG Aimers I trusted a local validation score, submitted, and got 415 points. I then rebuilt validation with the agents to match real submission conditions.
- **No number without a source.** In the newsletter repository I withdrew figures whose producing code no longer existed, and listed separately what was measured and what was not.
- **Human review has a defined place.** At Trip.com, opening hours, addresses and proper nouns were reviewed by a person. In the newsletter project, a cluster that cannot be cleaned up into one event is skipped, and a draft that fails three attempts is not saved.

## How I use AI coding agents

- What was delegated, in pieces: on the LG Aimers personal exploration track, experiment design and implementation, the root-cause report, and the row-independence audit of five submission packages; in the newsletter solo follow-up, the code, tests and 19 design decision records. Tools: Claude Code, opencode, Codex.
- How the output was checked: only versions that passed the new validation were submitted to the real leaderboard and scored. The row-independence audit used the official sample rows. CI tests on every push, a test that runs the whole LangGraph flow with a fake LLM, and synthetic perturbation measurement.
- Where I pushed back or stopped: rebutted the agent's judgment that 1,100 points was unrealistic with the leaderboard. Had the claimed overlap with team code checked by comparison and measurement, and rejected four candidates. Accepted the top local-validation recommendation as it was, got 415 points, and stopped all experiments. In the newsletter work, withdrew figures whose producing code no longer existed.
- Per-case tables: [LG Aimers](./projects/lg-aimers-agent-validation.en.md#where-ai-coding-agents-were-used), [newsletter](./projects/newsletter-langgraph.en.md#where-ai-coding-agents-were-used)

## Skills

The cases in parentheses are where each skill was used. "Integration" marks a technology where a teammate built the component and my work only connected to it.

- Languages and data: Python (Trip.com merge script, LG Aimers, newsletter), Pandas and NumPy (LG Aimers), SQL (newsletter), PostgreSQL with pgvector (newsletter, integration)
- Models and frameworks: PyTorch and Hugging Face Transformers (thesis), PEFT with LoRA, Prefix Tuning and Prompt Tuning (thesis), LangGraph (newsletter), CatBoost (feature engineering in LG Aimers), TabPFN (LG Aimers, feasibility check)
- AI coding agents: Claude Code (LG Aimers, newsletter solo follow-up), opencode (LG Aimers), Codex (LG Aimers final)
- Environment: Git and GitHub (newsletter, LG Aimers, the thesis fork), Linux (newsletter), Airflow (newsletter collection and generation batches, shared with teammates), Docker (Docker Compose in the newsletter solo follow-up, set up by an AI coding agent), FastAPI (newsletter, integration)

## Education and training

- Fudan University, Bachelor's degree in Information Management and Information Systems (Sep 2017 to Jun 2025)
- LG Aimers 9th Cohort, completed (LG AI Research, Jun to Sep 2026)
- Boostcamp AI Tech 8th Cohort, RecSys Track, completed (NAVER Connect Foundation, Sep 2025 to Feb 2026)
- Languages: Korean, Chinese (HSK Level 6), English

The text in this repository was drafted and edited with AI tools. Copyright in the text and diagrams belongs to the author, and the linked repositories follow their own licenses.

Last updated: 2026-10-11
