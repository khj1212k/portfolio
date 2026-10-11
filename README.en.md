[한국어](./README.md) | [English](./README.en.md)

# Heung Jun Kim — AI Engineer

[LinkedIn](https://www.linkedin.com/in/heung-jun-kim-46652031a) | [GitHub](https://github.com/khj1212k)

New-graduate AI engineer who sets the criteria first and checks what LLMs produce against them.

I built an LLM translation pipeline as a Trip.com intern and a LangGraph newsletter generation and evaluation pipeline as leader of a five-person team, and fine-tuned a 7B vision-language model for my graduation thesis.

When I hand implementation to AI coding agents, I still set the direction and the verification criteria myself and check the results under real conditions.

---

## Projects

### Trip.com LLM Translation Pipeline — I was translating attraction data by hand, one entry at a time

> Trip.com Content Center | Intern, sole owner of Korean data | Nov 2024 to Feb 2025

**Problem**: I was doing every Korean translation myself, one attraction at a time. Only 72.3% of the attractions in China rated 4.0 or higher had Korean information.

**Solution**: Proposed building an LLM translation pipeline in an internal document. Compared five LLMs on identical inputs to choose a model. Designed the prompt template and a review rule: check first the fields where an error harms a traveler.

**Result**: Raised Korean coverage of 9,755 attractions from 72.3% to 100%. Documented the prompt template and review criteria and shared them with the English, Japanese and Thai owners.

[Case study](./projects/tripcom-llm-translation.en.md) | Code and prompts are company material and not public

### LangGraph Newsletter Generation — One-pass drafts mixed separate events and still hallucinated

> Boostcamp AI Tech 8th final project | Leader of a five-person team, then solo follow-up | Jan to Feb 2026

**Problem**: When an LLM wrote a newsletter from a cluster of articles in one pass, separate events ended up in the same issue. Prompt tuning alone could not reliably prevent hallucination or missing key facts.

**Solution**: Split generation and evaluation into separate LangGraph nodes. Split clusters a second time and had an LLM check first whether a cluster is a single event. After the team project, delegated implementation to Claude Code and added a check that compares numbers and quotes with the source articles.

**Result**: Capped generation at three attempts; a draft that still fails evaluation is not saved. In the follow-up, the source check caught all 500 synthetic cases in which a number had been altered.

[Case study](./projects/newsletter-langgraph.en.md) | [Code and tests](https://github.com/khj1212k/pro-recsys-finalproject-recsys-01) | [ADR 0010](https://github.com/khj1212k/pro-recsys-finalproject-recsys-01/blob/1a42b46c6efa93431e1dd0eb5fff0083ce14d914/docs/adr/0010-faithfulness-gate-and-judge-v2.md)

### LG Aimers 9th Hackathon — The top local-validation combination collapsed on real submission

> LG Aimers 9th hackathon | Five-person team; GBDT models and features | Aug to Sep 2026

**Problem**: On a personal exploration track that I ran with AI coding agents alongside the team work, the combination that ranked first in local validation dropped to 415 points when submitted.

**Solution**: Stopped every experiment and had the cause written up first. Rebuilt validation with the agents to match real submission conditions, and submitted only the versions that passed.

**Result**: Raised the personal exploration track from 783.78 to 983.26 points; two calibration signals verified there were adopted in the team's final model. The team placed 17th of 1,030 teams and advanced to the final, for which 34 teams were selected.

[Case study](./projects/lg-aimers-agent-validation.en.md) | [DACON leaderboard](https://dacon.io/competitions/official/236743/leaderboard) | Code is private under competition rules

### Person Re-identification with a Vision-Language Model — Whether a VLM could do it had to be tested

> Fudan University graduation thesis | Individual research | Mar to May 2025

**Problem**: Whether a vision-language model could be used for person re-identification had to be tested by experiment. The GPU budget was limited: training all of a 7B model took two to three hours per epoch.

**Solution**: Recast re-identification as question answering over image pairs and fine-tuned LLaVA-Interleave-Qwen-7B. Compared full fine-tuning with three PEFT methods under matched total training time. Designed a two-stage transfer that reuses the LoRA weights as the starting point for Person Search training.

**Result**: Reached 97.05% same-person accuracy on Market-1501, and the two-stage transfer raised Person Search ID accuracy from 71.76% to 78.57%. The same research produced two first-author conference papers.

[Case study](./projects/vlm-person-reid-thesis.en.md) | [Training script fork](https://github.com/khj1212k/lmms-finetune)

---

## How I Use AI Coding Agents

**Delegated**: In the newsletter solo follow-up, the code, tests and design decision records. On the LG Aimers personal exploration track, experiment design and implementation, the root-cause report and the audit of the submission packages. Tools: Claude Code, opencode, Codex.

**Decided by me**: The direction and the verification criteria, when to submit and when to stop, and whether to accept a result. I used the leaderboard to rebut the agent's judgment that 1,100 points was unrealistic, and withdrew figures whose producing code no longer existed.

**How I checked**: Scores from real leaderboard submissions, CI tests on every push, a test that runs the whole LangGraph flow with a fake LLM, and synthetic perturbation measurements.

The newsletter and LG Aimers case studies each set out who did what under "Where AI coding agents were used".

---

## Other Experience

<details>
<summary><b>Two conference papers</b> — First author | 2025</summary>

- Kim, H. J., et al. (2025). "A Study of Person Re-identification Based on Vision-Language Multimodal Large Language Model." 31st International Conference on IT Applications and Management (ITAM-31), proceedings pp. 252-259.
- Kim, H. J., et al. (2025). "A Cost-Effective Training Method for Large Models in Pedestrian Search Task." 20th CCF Conference on Computer Supported Cooperative Work and Social Computing (ChineseCSCW 2025).
- Both papers report the graduation thesis research

</details>

<details>
<summary><b>Education and training</b> — Bachelor's degree at Fudan University, LG Aimers 9th, Boostcamp AI Tech 8th</summary>

- Fudan University, Bachelor's degree in Information Management and Information Systems (Sep 2017 to Jun 2025)
- LG Aimers 9th Cohort, completed (LG AI Research, Jun to Sep 2026)
- Boostcamp AI Tech 8th Cohort, RecSys Track, completed (NAVER Connect Foundation, Sep 2025 to Feb 2026)
- Languages: Korean, Chinese (HSK Level 6), English

</details>

<details>
<summary><b>VisuAI</b> — Walking-assistance web app for visually impaired users, three-person team | Dec 2022 to Jan 2023</summary>

- A web app that detects obstacles with the camera and announces them by voice
- The team fine-tuned YOLOv5-nano and converted it to TensorFlow.js to run in the browser
- [Repository](https://github.com/khj1212k/VisuAl_web)

</details>

---

## Technical Skills

| Area | Stack |
| --- | --- |
| **Languages** | Python, SQL |
| **AI/ML** | PyTorch, Hugging Face Transformers, PEFT (LoRA, Prefix Tuning, Prompt Tuning), LangGraph, CatBoost |
| **AI coding agents** | Claude Code, opencode, Codex |
| **Data** | Pandas, NumPy, Airflow |
| **Infra** | Git, GitHub, Linux, Docker |
| **Integrated with, not built** | PostgreSQL (pgvector), FastAPI |

---

The text in this repository was drafted and edited with AI tools. Copyright in the text and diagrams belongs to the author.
