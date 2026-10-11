[한국어](./README.md) | [English](./README.en.md)

# 김형준 (Heung Jun Kim)

신입 AI 엔지니어입니다. Trip.com 인턴으로 LLM 번역과 검수 흐름을, 5인 팀의 팀장으로 LangGraph 뉴스레터 생성과 평가 파이프라인을 만들었고, 졸업논문에서 7B 시각-언어 모델을 미세조정했습니다(제1저자 학술대회 논문 2편). 기준을 먼저 세우고, LLM과 AI 코딩 에이전트가 낸 결과를 그 기준으로 검증합니다.

구현을 AI 코딩 에이전트에 맡긴 곳은 사례마다 '에이전트가 한 것'과 '내가 정하고 확인한 것'으로 나눠 적었습니다([아래 절](#ai-코딩-에이전트를-쓰는-방식)).

밖에서 확인할 수 있는 것

- [뉴스레터 fork](https://github.com/khj1212k/pro-recsys-finalproject-recsys-01): 코드, tests/와 docs/adr/, CI 통과
- [DACON Phase 2 리더보드](https://dacon.io/competitions/official/236743/leaderboard): 17위 행(1,184.97753점)
- [학습 스크립트 fork](https://github.com/khj1212k/lmms-finetune): 2025-04-29 ~ 05-08 커밋 5개

연락: [LinkedIn](https://www.linkedin.com/in/heung-jun-kim-46652031a) | [GitHub](https://github.com/khj1212k)

## 한눈에 보기

대표 수치와 그 수치를 잰 조건입니다.

- **관광지 9,755곳의 한국어 커버리지 72.3%에서 100%**: Trip.com 인턴, 평점 4.0 이상 중국 관광지, 2024.11과 2025.02 비교
- **LangGraph 뉴스레터 생성과 평가 파이프라인**: 5인 팀 팀장. 팀 종료 뒤 fork에서 커밋 463개, 설계 결정 기록(ADR) 19건, CI 테스트는 푸시마다 실행되고 2026-10-06 기준 최신 커밋은 통과(구현은 AI 코딩 에이전트, 방향과 검증 기준은 직접)
- **개인 탐색 트랙 783.78점에서 983.26점**: LG Aimers 9기 해커톤. 로컬 검증 1위 조합이 제출에서 415점으로 떨어진 뒤, 검증을 실제 제출 조건에 맞춰 다시 세우고 통과한 버전만 제출해 받은 리더보드 점수(실험 구현은 AI 코딩 에이전트, 제출 기준과 중단은 직접). 팀 결과는 1,030팀 중 17위로, 34팀이 선발된 본선에 진출
- **동일인 판별 정확도 97.05%**: 졸업논문, Market-1501, 7B 시각-언어 모델의 전체 미세조정. 같은 연구로 제1저자 학술대회 논문 2편(ITAM-31, ChineseCSCW 2025)

| 프로젝트 | 푼 문제 | 내가 한 일 | 확인할 자료 |
| --- | --- | --- | --- |
| [Trip.com LLM 번역 파이프라인](./projects/tripcom-llm-translation.md)<br>인턴, 2024.11 ~ 2025.02 | 평점 4.0 이상 관광지의 한국어 정보가 72.3%뿐이고 번역은 사람이 한 건씩 하고 있었음 | LLM 번역 도입을 사내 문서로 제안, 틀리면 여행자가 피해를 보는 필드부터 검수하는 기준과 프롬프트 틀 설계, 5개 LLM을 같은 입력으로 비교, 병합 스크립트 작성 | 글과 다시 그린 흐름도. 회사 자료라 코드와 프롬프트 원문은 비공개 |
| [LangGraph 뉴스레터 생성](./projects/newsletter-langgraph.md)<br>5인 팀 팀장, 2026.01 ~ 2026.02. 이후 개인 후속 작업 | 여러 기사를 LLM에 한 번에 쓰게 하면 사건이 섞이고 환각과 누락을 매번 막지 못함 | 생성과 평가를 LangGraph 노드로 분리, 2단계 군집, 배치 주기 설계. 팀 종료 뒤 fork에서 원문 대조 검사 추가(구현은 AI 코딩 에이전트) | [코드와 테스트](https://github.com/khj1212k/pro-recsys-finalproject-recsys-01) CI 통과, [ADR 0010](https://github.com/khj1212k/pro-recsys-finalproject-recsys-01/blob/1a42b46c6efa93431e1dd0eb5fff0083ce14d914/docs/adr/0010-faithfulness-gate-and-judge-v2.md), [그래프 수준 테스트](https://github.com/khj1212k/pro-recsys-finalproject-recsys-01/blob/1a42b46c6efa93431e1dd0eb5fff0083ce14d914/tests/test_workflow_faithfulness_gates.py) |
| [LG Aimers 9기 해커톤](./projects/lg-aimers-agent-validation.md)<br>5인 팀, 2026.08 ~ 2026.09 | 로컬 검증 1위 조합이 제출에서 415점으로 급락 | 검증을 실제 제출 조건에 맞춰 다시 세우고 통과한 버전만 제출(구현은 AI 코딩 에이전트) | [DACON 리더보드](https://dacon.io/competitions/official/236743/leaderboard) 17위 행. 코드는 대회 규칙상 비공개 |
| [시각-언어 모델 행인 재식별 연구](./projects/vlm-person-reid-thesis.md)<br>개인 연구, 2025.03 ~ 2025.05 | 시각-언어 모델을 행인 재식별에 쓸 수 있는지, 제한된 GPU에서는 어떤 미세조정 방식을 골라야 하는지 | 7B 모델의 전체 미세조정과 PEFT 3종을 총 학습 시간을 맞춰 비교, 2단계 전이 설계 | [학습 스크립트 fork](https://github.com/khj1212k/lmms-finetune) 커밋 5개. 논문 2편은 서지 정보 |

그 밖의 프로젝트

- VisuAI (2022.12 ~ 2023.01, 3인 팀): 카메라로 찾은 장애물을 음성으로 알리는 시각장애인 보행 보조 웹앱. 팀이 YOLOv5-nano를 미세조정하고 TensorFlow.js로 변환해 브라우저에서 동작하게 했습니다.

## 읽는 순서

- 1분: 위 표
- 5분: 코드를 보려면 [뉴스레터 사례](./projects/newsletter-langgraph.md)의 4번과 fork의 [ADR 0010](https://github.com/khj1212k/pro-recsys-finalproject-recsys-01/blob/1a42b46c6efa93431e1dd0eb5fff0083ce14d914/docs/adr/0010-faithfulness-gate-and-judge-v2.md), [그래프 수준 테스트](https://github.com/khj1212k/pro-recsys-finalproject-recsys-01/blob/1a42b46c6efa93431e1dd0eb5fff0083ce14d914/tests/test_workflow_faithfulness_gates.py). 업무에 적용한 결과를 보려면 [Trip.com 사례](./projects/tripcom-llm-translation.md)
- 더 보려면: [LG Aimers 사례](./projects/lg-aimers-agent-validation.md)(AI 코딩 에이전트를 운영하고 검증한 일)와 [졸업논문 사례](./projects/vlm-person-reid-thesis.md)

직무에 따라 먼저 볼 곳입니다.

- AI 에이전트 직무: [뉴스레터 사례](./projects/newsletter-langgraph.md)의 1, 2, 4번(LangGraph 판정 노드, 재시도 상한, 원문 대조 검사), 그다음 [LG Aimers 사례](./projects/lg-aimers-agent-validation.md)(AI 코딩 에이전트 운영과 검증)
- LLM 응답 품질을 다루는 직무: [뉴스레터 사례](./projects/newsletter-langgraph.md)의 1번과 4번, [Trip.com 사례](./projects/tripcom-llm-translation.md)
- AX와 데이터 직무: [Trip.com 사례](./projects/tripcom-llm-translation.md), [뉴스레터 사례](./projects/newsletter-langgraph.md)의 3번, [LG Aimers 사례](./projects/lg-aimers-agent-validation.md)(정형 데이터와 검증)
- 모델 학습 직무: [졸업논문 사례](./projects/vlm-person-reid-thesis.md), [LG Aimers 사례](./projects/lg-aimers-agent-validation.md)의 GBDT와 피처

모든 사례 문서는 같은 순서입니다. 문제, 내가 한 일과 판단, 결과와 검증, 내 범위와 팀 범위(개인 연구인 졸업논문 문서는 논문 목록), 한계, 확인할 자료. 뉴스레터와 LG Aimers 문서에는 한계 앞에 'AI 코딩 에이전트를 쓴 부분' 절이 더 있습니다.

이 저장소는 사례 문서입니다. 코드는 각 문서의 '확인할 자료'에서 링크합니다. 코드를 열어 볼 수 있는 곳은 뉴스레터 fork와 졸업논문의 학습 스크립트 fork이고, Trip.com은 회사 자료, LG Aimers는 대회 규칙 때문에 글과 다시 그린 그림으로만 적었습니다.

## 논문

1. **Kim, H. J.**, et al. (2025). "A Study of Person Re-identification Based on Vision-Language Multimodal Large Language Model." 31st International Conference on IT Applications and Management (ITAM-31), 논문집 pp. 252-259.
2. **Kim, H. J.**, et al. (2025). "A Cost-Effective Training Method for Large Models in Pedestrian Search Task." 20th CCF Conference on Computer Supported Cooperative Work and Social Computing (ChineseCSCW 2025).

두 편 모두 [졸업논문 연구](./projects/vlm-person-reid-thesis.md)를 정리한 논문입니다.

## 일하는 방식

- **기준을 먼저 정합니다.** Trip.com에서는 수천 건을 모두 볼 수 없어, 틀리면 여행자가 피해를 보는 필드부터 검수하기로 정했습니다. 뉴스레터에서는 평가 기준을 통과하지 못한 초안은 3번 안에 고쳐지지 않으면 저장하지 않게 했습니다.
- **결과는 실제 조건에서 확인합니다.** LG Aimers에서 로컬 검증 점수를 믿고 제출했다가 415점을 받은 뒤, 검증을 실제 제출 조건에 맞춰 에이전트와 다시 세웠습니다.
- **근거를 댈 수 없는 수치는 싣지 않습니다.** 뉴스레터 저장소에서는 산출한 코드가 남아 있지 않은 수치를 철회하고, 측정한 것과 측정하지 않은 것을 나눠 적었습니다.
- **사람이 보는 자리를 정해 둡니다.** Trip.com에서는 운영시간, 주소, 고유명사를 사람이 검수했고, 뉴스레터에서는 한 사건으로 정리되지 않는 묶음은 건너뛰고 3번 미달 초안은 저장하지 않게 했습니다.

## AI 코딩 에이전트를 쓰는 방식

- 쪼개서 맡긴 것: LG Aimers 개인 탐색 트랙의 실험 설계와 구현, 원인 보고서, 제출 패키지 5종의 행 독립성 감사. 뉴스레터 개인 후속 단계의 코드, 테스트, 설계 결정 기록 19건. 도구는 Claude Code, opencode, Codex.
- 검증한 수단: 새 검증을 통과한 버전만 실제 리더보드에 제출해 점수 확인. 공식 샘플 행으로 한 행 독립성 감사. 푸시마다 도는 CI 테스트, 가짜 LLM으로 LangGraph 전체를 돌리는 테스트, 합성 변형 측정.
- 반박하거나 멈춘 곳: 1,100점이 비현실적이라는 에이전트의 판단을 리더보드로 반박. 팀 코드와 겹친다는 추정을 대조와 실측으로 확인하게 해 후보 4종 기각. 로컬 검증 1위 추천을 그대로 받아 415점을 받은 뒤 실험 전체 중단. 뉴스레터에서는 산출 코드가 남지 않은 수치를 철회.
- 사례별 표: [LG Aimers](./projects/lg-aimers-agent-validation.md#ai-코딩-에이전트를-쓴-부분), [뉴스레터](./projects/newsletter-langgraph.md#ai-코딩-에이전트를-쓴-부분)

## 기술

괄호 안은 그 기술을 쓴 사례입니다. 팀원이 만든 부분에 제 작업을 맞물려 쓰기만 한 기술은 '연동'이라고 적었습니다.

- 언어와 데이터 처리: Python(Trip.com의 병합 스크립트, LG Aimers, 뉴스레터), Pandas와 NumPy(LG Aimers), SQL(뉴스레터), PostgreSQL과 pgvector(뉴스레터, 연동)
- 모델과 프레임워크: PyTorch와 Hugging Face Transformers(졸업논문), PEFT의 LoRA, Prefix Tuning, Prompt Tuning(졸업논문), LangGraph(뉴스레터), CatBoost(LG Aimers의 피처 엔지니어링), TabPFN(LG Aimers, 적용 가능성 검토)
- AI 코딩 에이전트: Claude Code(LG Aimers, 뉴스레터 개인 후속 단계), opencode(LG Aimers), Codex(LG Aimers 본선)
- 개발 환경: Git과 GitHub(뉴스레터, LG Aimers, 졸업논문의 fork), Linux(뉴스레터), Airflow(뉴스레터의 수집과 생성 배치, 팀원과 공동), Docker(뉴스레터 개인 후속 단계의 Docker Compose, 구성은 AI 코딩 에이전트), FastAPI(뉴스레터, 연동)

## 학력과 교육

- Fudan University, Information Management and Information Systems 학사 (2017.09 ~ 2025.06)
- LG Aimers 9기 수료 (LG AI연구원, 2026.06 ~ 2026.09)
- 부스트캠프 AI Tech 8기 RecSys 트랙 수료 (네이버 커넥트재단, 2025.09 ~ 2026.02)
- 언어: 한국어, 중국어(HSK 6급), 영어

이 저장소의 글은 초안과 정리에 AI 도구를 썼습니다. 글과 그림의 저작권은 작성자에게 있고, 링크한 저장소는 각자의 라이선스를 따릅니다.

최종 수정: 2026-10-11
