[한국어](./vlm-person-reid-thesis.md) | [English](./vlm-person-reid-thesis.en.md)

# 시각-언어 모델 행인 재식별 연구 — 재식별에 쓸 수 있는지 실험으로 확인해야 했다

> Fudan University 졸업논문 | 개인 연구 | 2025.03 ~ 2025.05

두 이미지 속 인물이 같은 사람인지와 ID를 답하도록 LLaVA-Interleave-Qwen-7B를 파인튜닝한 학부 졸업논문 '멀티모달 대형 모델 기반 행인 인식 연구'입니다.

제한된 GPU에서 전체 파인튜닝과 PEFT 3종을 총 학습 시간을 맞춰 비교하고, ReID 단계의 LoRA 가중치를 Person Search 학습의 초기값으로 이어 썼습니다.

동일인 판별 정확도 97.05%(Market-1501)를 얻었고, 같은 연구로 제1저자 학술대회 논문 2편을 냈습니다.

---

## 한 일

```mermaid
flowchart TB
    subgraph S1["1단계 ReID"]
        A["Market-1501, MSMT17, RSTPReid"] --> B["이미지 쌍 질의응답 변환"]
        B --> C["전체 파인튜닝 1 epoch"]
        B --> D["LoRA 5 epoch"]
        B --> E["Prefix Tuning 5 epoch"]
        B --> F["Prompt Tuning 5 epoch"]
        C --> G["동일인 판별 정확도와 학습 비용 비교"]
        D --> G
        E --> G
        F --> G
    end
    subgraph S2["2단계 Person Search"]
        H["CUHK-SYSU 인물 이미지와 장면 이미지 쌍"] --> I["LoRA 가중치를 초기값으로 파인튜닝"]
        H --> J["ReID 단계 없는 기본 모델"]
        I --> K["판별 정확도와 ID 정확도 비교"]
        J --> K
    end
    D -- "LoRA 가중치" --> I
```

학습은 공개 프레임워크 lmms-finetune을 fork해 PyTorch, Hugging Face Transformers, PEFT, DeepSpeed로 Alibaba Cloud GPU에서 했습니다.

### 1. 과제 변환 — 모델은 두 이미지의 인물을 비교해 본 적이 없었다

**문제**: LLaVA-Interleave-Qwen-7B는 한 입력에 이미지 여러 장을 받을 수 있지만, 사전학습에서 두 이미지의 인물을 비교하는 과제를 본 적은 없었다.

**해결**: Market-1501, MSMT17, RSTPReid, CUHK-SYSU를 두 인물의 동일 여부와 ID를 답하는 질의응답 데이터로 변환. 같은 ID의 이미지로 양성 쌍을, 서로 다른 ID에서 무작위로 고른 이미지로 같은 수의 음성 쌍을 구성.

**결과**: 생성 모델은 답하지 않을 수도 있어 응답률을 지표에 추가. 판별 정확도는 평가 쌍 전체 기준과 응답한 쌍 기준으로 따로 집계.

### 2. 비교 설계 — 전체 파인튜닝은 PEFT보다 약 5배 오래 걸렸다

**문제**: 7B 모델 전체를 학습하면 epoch당 2~3시간이 걸리는 제한된 GPU 환경이었다. 예비 실험에서 전체 파인튜닝은 PEFT보다 학습 시간이 약 5배 길었다.

**해결**: 전체 파인튜닝을 성능 상한 기준선으로 두고 LoRA rank 8, Prefix Tuning 길이 32와 96, Prompt Tuning을 비교. epoch 수 대신 총 학습 시간을 맞춰 전체 파인튜닝은 1 epoch, PEFT는 5 epoch. 스케줄러, warmup 비율, 혼합 정밀도는 동일.

**결과**: Market-1501과 MSMT17에서 전체 파인튜닝이 가장 높았고, PEFT 중에서는 LoRA가 가장 높음. LoRA는 전체의 0.1% 미만인 파라미터로 MSMT17에서 전체 파인튜닝 정확도의 약 87%.

### 3. 학습 오류 — 오류 세 가지가 겹쳐 원인을 구분하기 어려웠다

**문제**: 멀티 GPU 설정 오류, 이미지 토큰 수 불일치, DeepSpeed의 sparse attention 충돌이 겹쳐 나와 어느 것이 원인인지 구분하기 어려웠다.

**해결**: 한꺼번에 해결하려던 방식을 버리고 로그를 처음부터 읽어 오류를 하나씩 분리. 각 오류가 다시 나타나는 가장 작은 실행 단위를 만들어 그 안에서 원인을 확인하고 수정.

**결과**: 세 문제를 차례로 해결한 뒤 학습을 끝까지 실행. 이미지 토큰 처리를 고친 커밋은 공개 fork에 남아 있음.

### 4. 2단계 전이 — 정확도는 전체 파인튜닝이 높았지만 자원이 제한돼 있었다

**문제**: Person Search는 잘라낸 인물 이미지와 전체 장면 이미지를 받아 그 인물이 장면에 있는지와 ID를 맞히는 과제다. 1단계에서는 전체 파인튜닝이 가장 정확했지만 자원이 제한된 환경을 전제한 연구였다.

**해결**: 1단계의 PEFT 중 Market-1501과 MSMT17에서 정확도가 가장 높았던 LoRA의 가중치를 CUHK-SYSU 학습의 초기값으로 재사용. ReID 단계를 거치지 않은 기본 모델과 같은 조건으로 비교.

**결과**: Person Search 판별 정확도 87.47%에서 92.86%, ID 정확도 71.76%에서 78.57%.

---

## 결과

| 항목 | 결과 | 조건 |
| --- | --- | --- |
| 전체 파인튜닝 | Market-1501 97.05%, MSMT17 96.15% | 1 epoch, 약 70억 파라미터, epoch당 2~3시간 |
| LoRA rank 8 | Market-1501 82.45%, MSMT17 83.76% | 5 epoch, 약 470만 파라미터, epoch당 30~40분 |
| Person Search 판별 정확도 | 87.47%에서 92.86% | CUHK-SYSU, 기본 모델 대비 +5.39%p |
| Person Search ID 정확도 | 71.76%에서 78.57% | CUHK-SYSU, 기본 모델 대비 +6.81%p |

---

## 더 보기

<details>
<summary><b>논문 2편</b></summary>

- Kim, H. J., et al. (2025). "A Study of Person Re-identification Based on Vision-Language Multimodal Large Language Model." 31st International Conference on IT Applications and Management (ITAM-31), 2025.10.26 ~ 10.30 개최, 논문집 pp. 252-259, ISSN 2334-0114.
- Kim, H. J., et al. (2025). "A Cost-Effective Training Method for Large Models in Pedestrian Search Task." 20th CCF Conference on Computer Supported Cooperative Work and Social Computing (ChineseCSCW 2025), 2025.11.07 ~ 11.09 개최.
- 두 편 모두 이 연구를 정리한 논문이고 게재가 확정됐으며 제1저자
- 1단계의 Market-1501과 MSMT17 수치는 ITAM 논문에, 2단계 수치는 ChineseCSCW 논문에도 수록

</details>

<details>
<summary><b>한계</b></summary>

- 수치는 평가 쌍 전체를 분모로 한 이미지 쌍 판별 정확도. 표준 검색 지표(mAP, Rank-1)는 없어 기존 ReID 방법과 직접 비교할 수 없음
- 설명 문장 없는 RSTPReid에서 전체 파인튜닝은 91.75%, LoRA는 42.17%. LoRA는 '같은 사람' 쪽 정밀도, 재현율, F1이 0%였고 원인은 분석하지 못함
- 총 학습 시간을 맞춘 비교여서 epoch 수, 학습률, weight decay가 방식마다 다름
- 숫자로만 표현한 ID 예측의 한계를 확인. 2단계 뒤에도 ID 정확도는 78.57%
- 공개 fork의 설정은 2025년 4월 말의 것으로 졸업논문의 최종 설정과 다름. 평가 코드와 체크포인트는 공개 저장소에 없음

</details>

<details>
<summary><b>확인할 자료</b></summary>

- [학습 스크립트 fork](https://github.com/khj1212k/lmms-finetune): 2025-04-29 ~ 05-08 커밋 5개. 원저장소는 zjysteven/lmms-finetune
- [loader 수정 커밋](https://github.com/khj1212k/lmms-finetune/commit/a4e4dc676e7ace9de80096132c0d692d400c7964), [collator 수정 커밋](https://github.com/khj1212k/lmms-finetune/commit/5ba914e99cf8b015df32d76d67dc38e4f50ce3fe): 이미지 토큰 처리 수정
- [llava_msmt17.sh](https://github.com/khj1212k/lmms-finetune/blob/541c909e6c2a5c66872e64d9d0ae1d5aaa1191e2/llava_msmt17.sh): MSMT17 학습 실행 스크립트
- [데이터 변환 커밋](https://github.com/khj1212k/lmms-finetune/commit/997a7ab60165abc1ee4368e8324214aecd643f29): 이미지 쌍 질의응답 변환 노트북과 결과 파일
- 변환 예시: 질문 `<image> Are these two people the same?<image> Also, guess the person-ID of each image.` 답 `no, id1: 178, id2: 864`
- 수치는 졸업논문 최종본의 표 3.1, 표 3.3, 표 3.4의 값. 인물 이미지, 논문 전문, 졸업논문 본문은 싣지 않음

</details>

---
