# [006] Scalable Qualitative Coding with LLMs: Chain-of-Thought Reasoning Matches Human Performance in Some Hermeneutic Tasks

> 분류: `C. 방법론 참고`
> 키워드: LLM 질적 코딩, 내용분석, 코드북 재설계, chain-of-thought, 코더 간 신뢰도(Cohen's κ), GPT-4, GPT-3.5, zero-shot, 계산적 근거이론
> 읽은 날짜: 2026-11-11 · 상태: 📖 정독 완료
>
> ⚠️ **판본 주의**: 이 노트는 **arXiv 프리프린트(arXiv:2401.15170v2, 2024.02.12)**를 읽고 정리한 것임. 이후 학술지판인 Dunivin (2025) "Scaling hermeneutics: a guide to qualitative coding with LLMs for reflexive content analysis", *EPJ Data Science* 14:28은 내려받지 못해 읽지 않았음. 학술지판은 제목부터 다르므로 데이터, 실험 설정, 수치, 권고 사항이 이 노트와 다를 수 있음. 학술지판을 인용할 때는 원문을 다시 확인할 것.
>
> 📌 **아카이브에 넣은 이유**: 아카이브 [001](001_mofa-china-discourse-llm-coding.md)(황수영 2026, 외교부 대중국 담론의 LLM 보조 코딩)이 따른 LLM 질적 코딩 절차의 **방법론 출처**(Dunivin 2025)임. 001은 일치도 지표 없이 LLM을 1차 분류에 썼는데, 이 논문은 같은 종류의 작업을 사람 골드 스탠더드 대비 Cohen's κ로 평가하고, 코드북을 LLM용으로 고치는 구체적 원칙을 제시함. 북한 매체·대북 문서 코딩(001 아이디어 1–3)을 설계할 때 그대로 가져다 쓸 수 있는 기준점.

## 1. 서지 정보

| 항목 | 내용 |
|---|---|
| 저자 (소속) | Zackary Okun Dunivin (Indiana University: Center for Complex Networks and Systems Research, Luddy School of Informatics, Computer Science, and Engineering; Department of Sociology) |
| 제목 (국문) | LLM을 이용한 확장 가능한 질적 코딩: 일부 해석학적 과제에서 chain-of-thought 추론이 인간 수준에 이른다 (비공식 번역) |
| 제목 (영문) | Scalable Qualitative Coding with LLMs: Chain-of-Thought Reasoning Matches Human Performance in Some Hermeneutic Tasks |
| 게재 형태 | arXiv 프리프린트, arXiv:2401.15170v2 [cs.CL], 2024.02.12 (이 노트의 근거 판본) |
| 쪽수 | 10쪽 (본문 1–9, 참고문헌 9–10). 본문은 코드북 원본·GPT용 수정본 전체와 프롬프트 전문이 부록에 있다고 하나, v2 PDF에는 부록이 포함되어 있지 않음 |
| 학술지판 | Dunivin, Z. O. (2025). Scaling hermeneutics: a guide to qualitative coding with LLMs for reflexive content analysis. *EPJ Data Science* 14, 28. doi:10.1140/epjds/s13688-025-00548-8 (**원문 미확인**. 서지는 아카이브 001 노트에서 가져옴) |
| DOI (프리프린트) | 원문에 없음 (arXiv 식별자만 있음) |
| 코드·데이터 | https://osf.io/k4fg9 (p. 1, 각주) |
| 연구 지원 | 원문에 없음. 사사에는 원고 논평자들과 코드북 공동 개발자 Tania Ravaei를 언급 (p. 9) |

```bibtex
@misc{dunivin2024scalable,
  author        = {Dunivin, Zackary Okun},
  title         = {Scalable Qualitative Coding with {LLMs}: Chain-of-Thought Reasoning Matches Human Performance in Some Hermeneutic Tasks},
  year          = {2024},
  eprint        = {2401.15170},
  archivePrefix = {arXiv},
  primaryClass  = {cs.CL},
  note          = {Version 2, 12 Feb 2024}
}

@article{dunivin2025scaling,
  author  = {Dunivin, Zackary Okun},
  title   = {Scaling hermeneutics: a guide to qualitative coding with {LLMs} for reflexive content analysis},
  journal = {EPJ Data Science},
  year    = {2025},
  volume  = {14},
  pages   = {28},
  doi     = {10.1140/epjds/s13688-025-00548-8},
  note    = {원문 미확인}
}
```

## 2. 한 줄 요약
W.E.B. Du Bois를 다룬 뉴욕타임스 단락을 9개 사회사적 코드로 코딩하는 과제에서, **코드별 개별 프롬프트 + 근거(rationale)를 먼저 쓰게 하는 CoT** 설정의 GPT-4는 사람 골드 스탠더드 대비 평균 κ = 0.68, 9개 중 8개 코드에서 κ ≥ 0.6을 기록했지만 GPT-3.5는 평균 κ = 0.34에 그쳤다 (p. 1, 7).

## 3. 내용 요약

**연구 질문**
1. 최신 LLM(GPT-4)이 문단 길이의 인문·사회사적 텍스트에 대해 사람 수준의 질적 코딩을 할 수 있는가?
2. 사람용으로 만든 코드북을 LLM용으로 어떻게 고쳐야 하는가?
3. 프롬프트 구성(근거 제시 요구 여부, 코드별 개별 과제 vs. 코드북 전체 한 번에)이 코딩 품질에 어떤 영향을 주는가?

**저자가 내세운 기여 (p. 2)**: (1) GPT-4로 질적 코딩을 자동화한 첫 방법론적 보고 (2) 여러 절이 얽힌 긴 단락에서 LLM이 사람 수준 성능을 낼 수 있다는 "결정적 증거" (3) 코딩 결정을 정당화하게 할 때(CoT) 해석이 더 정확해짐

### 3-1. 방법론
- **코드북 개발 → LLM용 재설계 (p. 3–6)**
  - 먼저 일반적인 방식으로 사람용 코드북을 만듦: 탐색적 읽기 → 코드 정의·수정 → 테스트셋에서 높은 코더 간 신뢰도가 나올 때까지 반복
  - 이어서 학습용(training) 단락에서 LLM 성능을 보며 코드 설명을 고침. Nelson의 **계산적 근거이론(Computational Grounded Theory)** 의 반복 순환(정의–평가–수정)을 따름
  - LLM이 낸 **근거(rationale)를 진단 도구로 씀**: 근거에서 반복적으로 드러나는 오해를 보고 해당 코드 정의를 고친 뒤 다시 테스트함. 고치면 다른 단락에서 새 오류가 생기기도 했다고 솔직하게 적음
- **LLM용 코드북 수정에서 얻은 원칙 (표 3, p. 8)**

| 구분 | 원칙 | 내용 |
|---|---|---|
| 과제 지시 | Prompt for Rationale | 코딩 결정의 근거를 쓰게 하면 정확도가 오름 |
| | One Task Per Code | 코드마다 별도 과제로 주면 정확도가 오름 |
| | Brevity | 짧은 과제 설명일수록 충실히 수행됨 |
| | Structured Output | 출력 형식을 지정해 응답을 균일하게 함 |
| 코드 정의 | Word Choice | 의미가 큰 단어 하나를 LLM의 내장 온톨로지에 맞게 바꿈 |
| | Clause Order | 앞쪽에 둔 조건절이 더 잘 지켜짐 |
| | Mandates/Prohibitions | 둘 다 통하지만 "하라"가 "하지 마라"보다 쉬움 |
| | Code Titles | 정의를 그대로 두고 제목만 바꿔도 효과가 큼 |
| | Interpretation Scope | 해석이 너무 좁거나 넓으면 "implicit/explicit" 같은 단어로 조절 |
| CoT 순서 | 1–4 | ① 역할 부여 ② 코드 정의 ③ 근거 요청 ④ 일정한 형식으로 결정 출력 |

- 대표 사례 (p. 4–5)
  - "Academic Repute"라는 제목을 GPT가 '학자로서의 명성'으로 계속 오해함. 정의는 그대로 두고 제목만 **"Out of the Mouth of Academics"**로 바꾸자 성능이 크게 오름
  - "Social/Political Activism"은 GPT가 사회 비판을 활동(activism)으로 보지 않아 "Social/Political Advocacy"로 바꿈 (그림 2)
- **프롬프트 구성 (p. 6)**: 과제 설명은 system prompt, 단락은 user prompt로 줌. 전 과정 **zero-shot**. 문학·역사적 긴 단락에서는 예시를 주면 LLM이 예시 내용을 끌어다 써서 판단을 방해했고, Full Codebook 프롬프트에서는 예시가 프롬프트를 지나치게 늘려 성능이 떨어졌다고 함
- **기계 판독 출력**: "Codes Applied:" 같은 태그 뒤에 코드 목록을 쓰게 하고, 그 뒤에 아무것도 쓰지 말라고 지시함 (그림 1, p. 4–6)
- **추론 설정**: temperature 0, top_p 1 (저자는 이를 GPT API 기본값이라고 서술, p. 6). 모델 스냅샷·버전명은 원문에 없음

### 3-2. 데이터셋
- **주제**: 학자이자 활동가인 W.E.B. Du Bois가 뉴스 매체에서 시대별로 어떻게 묘사되었는지 (저자 본인의 연구를 사례로 삼음, p. 3)
- **출처·기간**: *New York Times* 기사 (1970–2023) 중 "Du Bois"를 포함하는 연속 단락을 자동 추출
- **규모**: 단락 **232개** (무작위 표본). 평균 **94단어(σ = 70)**, 평균 **3.75문장(σ = 2.88)** (p. 3)
- **평가셋**: 골드 스탠더드 단락 **111개** (표 2, p. 7). 골드 스탠더드는 개발 전 단계에서 LLM에게 숨겨 둠 (p. 6). 나머지 단락이 학습(개발)용으로 쓰였는지, 그 정확한 수는 원문에 명시되어 있지 않음
- **코드 체계 (표 1, p. 3)**: 3개 범주, 9개 코드, 비배타적(한 단락에 여러 코드 가능)
  - Du Bois 성격 규정: Scholar, Activist
  - 일반 주제: Monumental Memorialization, Mention of Scholarly Work, Social/Political Advocacy
  - 정전화 과정: Coalition Building, Out of the Mouth of Academics, Out of the Mouth of Activists, Collective Synecdoche
- **라벨링**: 사람 코더가 만든 골드 스탠더드. 몇 명이 코딩했는지, 사람 간 κ가 얼마였는지는 원문에 수치로 나오지 않음 ("high intercoder reliability with a test set"라고만 서술, p. 3)
- **공개 여부**: 코드와 데이터를 OSF(osf.io/k4fg9)에 공개한다고 밝힘 (p. 1)

### 3-3. 실험
- **설정**: 2 × 2 + 1 비교
  - GPT-4 × {Per Code, Full Codebook} × {CoT, No CoT}
  - GPT-3.5 × Per Code × {CoT, No CoT}
  - "Per Code" = 코드 하나당 별도 프롬프트, "Full Codebook" = 9개 코드를 한 프롬프트로
- **비교 대상**: 사람 골드 스탠더드 (사람 코더 간 일치도 수치나 지도학습 분류기 같은 기계 기준선은 없음)
- **지표**: 코드별 **Cohen's κ** (골드 대비). 0.6 이상을 substantial, 0.75 이상을 excellent로 봄 (p. 2). 두 값의 차이가 0.02 이하면 같은 것으로 취급 (표 2 설명)
- **주요 결과 (표 2, p. 7, 골드 111단락)**

| 코드 | 양성 수 | GPT-4 Per Code CoT | GPT-4 Per Code No CoT | GPT-4 Full CoT | GPT-4 Full No CoT | GPT-3.5 CoT | GPT-3.5 No CoT |
|---|---|---|---|---|---|---|---|
| Scholar | 27 | 0.61 | 0.52 | 0.59 | 0.42 | 0.29 | 0.21 |
| Activist | 23 | **0.81** | 0.65 | 0.67 | 0.62 | 0.39 | 0.32 |
| Monumental Memorialization | 13 | **1.00** | 0.91 | 0.75 | 0.48 | 0.29 | 0.31 |
| Mention of Scholarly Work | 24 | 0.71 | 0.69 | 0.52 | 0.44 | 0.33 | 0.39 |
| Social/Political Advocacy | 51 | 0.64 | 0.60 | 0.60 | 0.60 | 0.55 | 0.51 |
| Coalition Building | 9 | 0.60 | 0.44 | 0.43 | 0.13 | 0.33 | 0.17 |
| Out of the Mouth of Academics | 30 | 0.63 | 0.65 | 0.65 | 0.62 | 0.37 | 0.33 |
| Out of the Mouth of Activists | 11 | 0.30 | 0.09 | 0.34 | 0.18 | 0.21 | 0.09 |
| Collective Synecdoche | 26 | 0.79 | 0.78 | **0.81** | 0.71 | 0.27 | 0.27 |
| **평균** | 24 | **0.68** | 0.59 | 0.60 | 0.46 | 0.34 | 0.29 |

- **핵심 수치 정리**
  - GPT-4 (Per Code, CoT): 9개 중 8개 코드가 κ ≥ 0.6, 3개(Activist 0.81, Monumental Memorialization 1.00, Collective Synecdoche 0.79)가 "사람 수준" (p. 1–2, 6)
  - GPT-3.5: 평균 κ = 0.34, 최대 0.55 (p. 1). GPT-4가 가장 잘한 3개 코드에서 GPT-3.5는 오히려 자기 평균보다 약간 낮음 (0.32 vs. 0.34, p. 6)
  - **CoT 효과**: GPT-4 Per Code 0.59 → 0.68, Full Codebook 0.46 → 0.60. 한 가지 예외를 빼면 모든 코드·조건에서 CoT가 같거나 높음 (p. 7)
  - **Per Code 효과**: CoT 기준 0.68 vs. 0.60 (p. 2). 사람 수준 3개 코드 중 2개에서 Per Code가 크게 앞섰고, Mention of Scholarly Work와 Coalition Building에서도 크게 앞섬. 나머지 4개는 비슷함 (p. 7)
  - **Out of the Mouth of Activists**는 어떤 설정에서도 잘 되지 않음 (최고 0.34)
- **정성 관찰 (p. 8)**: GPT-4는 사람처럼 경계 사례, 특히 암묵적 정보가 필요한 판단에서 어려움을 겪음. 다른 GPT-4 인스턴스를 비평가(critic) 모델로 쓰면 경계 사례를 스스로 알아채고 명백한 실수를 고치는 경우가 많았다고 함 (일화적 관찰이며 정량 평가는 없음)

### 3-4. 결론
- 질적 코딩을 많이 하는 연구자에게 LLM 자동 코딩은 "지금 이미" 고려할 만한 선택지라고 권고함 (p. 8)
- 같은 코드북 안에서도 코드별 품질 차이가 크므로, 분야·과제마다 성공 여부가 다를 것. 인문학적·"부드러운" 과제가 더 어려울 것으로 봄 (p. 7)
- 벤치마크 개발을 적극 주장하지는 않으며, 각자 비슷한 시도를 조사하고 직접 실험해 보라고 권함 (p. 7)
- 실무 권고 (p. 8)
  - 인터뷰처럼 **데이터가 작으면 손으로 코딩하는 편이 효율적**
  - 오류가 경계 사례에만 몰리고 체계적 편향이 없다면, 표본을 크게 늘릴 수 있어 노이즈를 감수할 만함
  - 불확실한 코드는 사람이 검토하는 human-in-the-loop 방식이나, 2단계 자동 성찰·수정(reflect-and-revise) 방식이 이미 가능해 보임
  - 연구자 혼자 스크립트를 짜기보다 학생·데이터과학자와 협업할 것

## 4. 해당 연구의 한계점

**저자가 밝힌 한계**
- 단일 코드북 안에서도 해석 품질이 코드마다 크게 다름. 다른 분야·과제로 일반화할 때 성공 여부와 코드북 전환 난이도가 크게 다를 것 (p. 7)
- GPT-4도 경계 사례, 특히 암묵적 정보가 필요한 판단에서 어려움을 겪음 (p. 8)
- "노이즈를 감수할 수 있다"는 주장은 **오류가 경계 사례에만 있고 체계적으로 편향되지 않았다는 가정**에 기댐 (p. 8)
- 기본 system prompt를 쓰고 과제 설명과 단락을 함께 user prompt로 주는 방식은 검토하지 않음 (p. 6)
- few-shot이 대부분의 코딩 과제에는 도움이 될 것이라며, zero-shot 선택이 이 사례(문학·역사적 텍스트)에 한정된 것일 수 있음을 인정함 (p. 6)
- 프롬프트를 시스템적으로 시험하고 전체 데이터에 적용하려면 Python 등 프로그래밍 역량이 필요함 (p. 8)

**내가 본 한계**
- **"사람 수준"의 기준이 비어 있음**: 제목과 초록은 "human performance"를 말하지만, 골드 스탠더드를 만든 사람 코더끼리의 κ를 수치로 보고하지 않음. 그래서 GPT-4의 0.68이 사람 간 일치도와 비교해 어느 정도인지 판단할 수 없음. "사람 수준"은 사실상 κ 임계값(0.6/0.75)을 넘었다는 뜻에 가까움
- **임계값 표기가 일관되지 않음**: 초록은 excellent를 "κ ≥ 0.79"로, 본문 핵심 발견은 "0.75 threshold"로 씀 (p. 1, 2). 3개 코드 중 최저값(0.79)에 맞춰 기준을 적은 것처럼 읽힐 수 있음
- **표본이 작고 불확실성 보고가 없음**: 골드 111단락, 코드별 양성 사례 9–51개. Coalition Building(9개), Out of the Mouth of Activists(11개)처럼 희소한 코드에서는 단락 한두 개로 κ가 크게 흔들림. 신뢰구간, 부트스트랩, 반복 실행 간 분산이 없어 "0.59 → 0.68" 같은 차이가 통계적으로 얼마나 확실한지 알 수 없음
- **도메인이 하나뿐임**: 영어, 한 매체(NYT), 한 인물, 한 코드북. 코드북을 만든 사람이 LLM용 수정도 하고 평가도 설계함. 저자 스스로 "softer" 과제의 어려움을 말하지만 다른 코드북으로 검증하지 않음
- **비교 기준선이 없음**: 지도학습 분류기(예: 파인튜닝한 BERT 계열), 오픈 모델, 사람 코더 1인 대비 성능 비교가 없음. GPT-4가 "가능하다"는 것은 보여 주지만 "다른 방법보다 낫다"는 것은 보여 주지 않음
- **재현성 정보가 부족함**: GPT-4/GPT-3.5의 정확한 스냅샷, 실험 날짜, 비용이 없음. 폐쇄형 모델은 시간이 지나면 바뀌거나 사라짐. 또 저자는 temperature 0을 API 기본값이라고 적었는데, 내가 아는 한 OpenAI API의 기본 temperature는 1이라 실제로 어떤 값이 쓰였는지 OSF 코드로 확인할 필요가 있음 (원문 외 지식)
- **CoT 효과의 원인이 분리되지 않음**: CoT 조건은 근거 작성, 출력 길이, 출력 형식이 함께 바뀜. "근거를 먼저 쓰는 것" 자체의 효과인지, 형식·길이 차이 때문인지 구분하는 대조 조건(예: 결정 후 근거 쓰기)이 없음
- **개발–평가 분리의 엄밀성**: 골드는 숨겼다고 하지만, 여섯 가지 설정을 모두 같은 111단락에서 비교했고 프롬프트 개발 단락 수도 명시되지 않음. 학습용 단락에서 반복 수정한 프롬프트가 그 단락들에 과적합되었을 가능성을 별도로 점검하지 않음
- **부록이 빠져 있음**: 본문이 프롬프트 전문과 코드북 두 판본을 "appendix"에서 보라고 하지만 v2 PDF(10쪽)에는 부록이 없음. OSF 자료로만 확인 가능함
- **북한 연구에 옮길 때의 간극**: 영어 단락과 GPT-4 기준 결과라서, 한국어·북한어 텍스트, 선전 수사처럼 의도된 과장이 많은 텍스트, 정치적으로 민감한 주제에서 같은 κ가 나올지는 전혀 알 수 없음. 모델의 정치적 편향이 "경계 사례 노이즈"가 아니라 **체계적 오류**로 나타날 수 있어, 저자의 "노이즈 감수" 논리가 성립하지 않을 수 있음

## 5. 추가로 읽을 유사 논문

✅ = 웹에서 서지 확인 · 📎 = 본 논문의 참고문헌에서 가져옴 (원문 미확인)

| 논문 | 왜 읽나 | 확인 |
|---|---|---|
| Chew, R., Bollenbacher, J., Wenger, M., Speer, J., & Kim, A. (2023). LLM-assisted content analysis: Using large language models to support deductive coding. arXiv:2306.14924 | 저자가 코드북 전환 절차(LACA)를 "더 자세히" 다룬 연구로 추천함. 3개 데이터셋 19개 과제. 001 같은 연역적 코딩 설계에 바로 참고 | 📎 |
| Xiao, Z., Yuan, X., Liao, Q. V., Abdelghani, R., & Oudeyer, P.-Y. (2023). Supporting qualitative analysis with large language models: Combining codebook with GPT-3 for deductive coding. *IUI '23 Companion* | GPT-3 기반 선행 연구 (κ = 0.61, 0.38). few-shot이 도움이 된다는 반대 사례 | 📎 |
| Nelson, L. K. (2017). Computational Grounded Theory: A methodological framework. *Sociological Methods & Research* 49(1), 3–42 | 본 논문의 반복적 코드 개발 절차가 따르는 방법론 틀 | 📎 |
| Ollion, E., Shen, R., Macanovic, A., & Chatelain, A. (2023). ChatGPT for text annotation? Mind the hype! SocArXiv, doi:10.31235/osf.io/x58kn | 20편의 경험 연구를 검토해 "엇갈린 결과"를 보고한 비판적 리뷰. 001처럼 LLM 코딩을 쓸 때 반론으로 함께 읽을 것 | 📎 |
| Vach, W. & Gerke, O. (2023). Gwet's AC1 is not a substitute for Cohen's kappa – A comparison of basic properties. *MethodsX* 10, 102212 | 희소 코드에서 어떤 일치도 지표를 써야 하는지. 저자가 Chew et al.의 AC1 사용을 비판한 근거 | 📎 |
| Wei, J. et al. (2022). Chain-of-thought prompting elicits reasoning in large language models. *NeurIPS* 35, 24824–24837 | CoT의 원전 | 📎 |
| Lin, Z., Trivedi, S., & Sun, J. (2023). Generating with confidence: Uncertainty quantification for black-box large language models. arXiv:2305.19187 | 저자가 권하는 "불확실한 코드만 사람이 검토" 워크플로를 구현할 때 필요한 불확실성 추정 방법 | 📎 |
| Paul, D. et al. (2023). REFINER: Reasoning feedback on intermediate representations. arXiv:2304.01904 | 저자가 일화적으로 관찰한 critic 모델 기반 성찰·수정 워크플로의 근거 | 📎 |
| 아카이브 [001](001_mofa-china-discourse-llm-coding.md) 황수영 (2026) | 이 논문(학술지판)을 방법 근거로 삼은 응용 사례. κ 없이 경계 사례 40건만 검토한 001의 설계를 이 논문 기준으로 다시 평가할 수 있음 | (아카이브) |

## 6. NLP 측면 연구 아이디어

### 아이디어 1. 코드 제목·정의 문구와 북한어 표기에 대한 LLM 코딩의 민감도 감사
- **문제 정의**: 이 논문은 코드 **제목 하나만 바꿔도** 성능이 크게 바뀐다고 보고함 (p. 5). 그렇다면 북한 텍스트 코딩에서는 (a) 코드 문구의 사소한 변화(예: '적대' vs. '비난', '괴뢰' 같은 북한 용어를 정의에 넣는지 여부)와 (b) 입력 텍스트의 **북한식 표기**(립장, 로동, 련합 등)가 판정을 얼마나 뒤집는지 체계적으로 측정함
- **데이터**: 로동신문 사설·조선중앙통신 기사 단락 300–500개와 아카이브 001식 코드북(행위자 역할·태도). 같은 단락의 남한식 표기 정규화판을 만들어 쌍으로 구성 (아카이브 [005](005_north-korean-mt-through-south-korean.md)의 남북 표기 대응을 활용)
- **방법**: 코드 정의마다 의미를 보존하는 변형(동의어 치환, 절 순서 변경, 제목 변경, do/do not 전환)을 만들어 같은 단락에 적용. 원문 표기판과 정규화판에도 각각 적용. 여러 LLM(상용·오픈)과 CoT 유무를 교차
- **평가**: 라벨 뒤집힘 비율(flip rate), 변형 간 κ, 골드 대비 κ의 변화폭. 북한식 표기만으로 판정이 바뀌는 사례는 따로 정성 분석
- **기대 기여**: LLM 코딩 결과가 연구자의 "문구 선택"과 텍스트의 "방언 표기"에 얼마나 의존하는지 보여 주는 감사 프로토콜. 북한학 LLM 코딩 논문이 함께 보고해야 할 강건성 지표를 제안

### 아이디어 2. 북한 매체 텍스트에서 Dunivin 설계 재현: Per Code × CoT × 모델, 사람 간 κ 기준선 포함
- **문제 정의**: 이 논문의 2 × 2 설계를 한국어 북한 텍스트에 그대로 재현하되, 원 논문에 없는 **사람 간 κ 기준선과 신뢰구간**을 넣어 "사람 수준" 주장을 제대로 검증함. 아카이브 001 아이디어 3(북한 텍스트 LLM 코더 신뢰도 감사)을 구체적 실험 설계로 만든 것
- **데이터**: 로동신문·조선중앙통신 단락과 통일부·외교부 대북 문서 단락을 같은 수로 표집. 사람 코더 2–3명이 독립 코딩해 골드와 사람 간 κ를 함께 구축. 희소 코드는 층화 표집으로 양성 사례를 최소 30개 이상 확보
- **방법**: {Per Code, Full Codebook} × {CoT, No CoT, 결정 후 근거 쓰기} × {상용 LLM 2종, 한국어 오픈 LLM 1–2종}. "결정 후 근거" 조건을 넣어 CoT 효과가 근거 선행 때문인지 형식·길이 때문인지 분리. 반복 실행(시드·날짜)으로 분산 측정
- **평가**: 코드별 κ와 부트스트랩 95% CI, LLM–사람 κ와 사람–사람 κ의 차이. 남한 문서와 북한 문서 사이의 성능 차이를 따로 보고
- **기대 기여**: 북한 텍스트에서도 "CoT + Per Code" 권고가 유효한지에 대한 첫 근거. 프롬프트·코드북·골드 데이터를 공개해 재사용 가능한 벤치마크로 만듦

### 아이디어 3. 불확실성·비평가 모델 기반 선택적 사람 검토: 북한 문서 코딩의 비용–정확도 곡선
- **문제 정의**: 저자는 불확실한 코드만 사람이 검토하는 워크플로가 "이미 가능해 보인다"고만 했고 정량 평가는 하지 않음 (p. 8). 아카이브 001은 경계 사례 40건 중 28건(70%)이 수정됨. 어떤 사례를 사람에게 넘겨야 하는지 자동으로 고르는 방법과, 검토 예산에 따라 최종 정확도가 어떻게 오르는지를 측정함
- **데이터**: 아이디어 2의 골드 데이터 (또는 001과 같은 외교 문서에 북한 관련 문서를 추가)
- **방법**: 검토 대상 선정 기준을 비교함: (a) 여러 번 샘플링한 응답의 불일치(self-consistency) (b) 언어화된 확신도 (c) 별도 critic LLM이 경계 사례로 표시한 것 (d) 무작위. 선정된 사례만 사람 판정으로 바꾸고 나머지는 LLM 판정 유지
- **평가**: 검토 비율(5%, 10%, 20%, ...)별 골드 대비 κ 곡선. 기준별로 같은 κ에 도달하는 데 드는 사람 검토 건수. 정치적으로 민감한 범주(예: 대남·대미 적대 표현)에서 오류가 체계적으로 몰리는지 따로 분석
- **기대 기여**: "LLM 1차 분류 → 사람 재검토"라는 001식 절차를 정량적으로 설계·보고하는 방법. 작은 연구팀이 북한 매체 아카이브 전체를 코딩할 때 필요한 사람 검토량을 미리 추정할 수 있음

## 7. 메모 · 인용하고 싶은 문장
- "Agreement improves when the LLM provides rationale for code assignments: µ(κ) = 0.68 vs. µ(κ) = 0.59." (p. 2)
- "altering the title of the code to the far more literal 'Out of the Mouth of Academics' dramatically improved performance, even when paired with the original code description" (p. 5) — 코드북 설계에서 가장 실용적인 교훈
- "this assumes that error is restricted to edge cases and is not otherwise systematically biased" (p. 8) — 북한 텍스트처럼 모델 편향이 우려되는 영역에서는 바로 이 가정을 검증해야 함
- "We also advise against automated coding where datasets are small, as in interviews" (p. 8) — 001처럼 175건 규모의 연구에 LLM을 쓸 때 정당화가 필요한 지점
- 학술지판(EPJ Data Science 2025)을 구하면 확인할 것: 데이터·모델 변경 여부, 사람 간 κ 보고 여부, 부록(코드북·프롬프트 전문) 포함 여부, 제목이 "reflexive content analysis"로 바뀐 이유
