# [003] Automated Claim–Evidence Extraction for Political Discourse Analysis: A Large Language Model Approach to Rodong Sinmun Editorials

> 분류: `B. AI·NLP로 북한(관련 담론) 분석`
> 키워드: 로동신문 사설, 주장–근거(claim–evidence) 추출, Chain-of-Thought 프롬프팅, LLM-as-a-judge, G-Eval, 해석적 환각(interpretive hallucination), Human-in-the-Loop, EXAONE
> 읽은 날짜: 2026-11-11 · 상태: 📖 정독 완료

## 1. 서지 정보

| 항목 | 내용 |
|---|---|
| 저자 (소속) | 최규리(Gyuri Choi), 김한샘(Hansaem Kim, 교신저자) — 연세대학교 언어정보학 협동과정 (Interdisciplinary Graduate Program of Linguistics and Informatics, Yonsei University) |
| 제목 (국문) | (국문 제목 없음) 정치 담론 분석을 위한 주장–근거 자동 추출: 로동신문 사설에 대한 LLM 접근 (임의 번역) |
| 제목 (영문) | Automated Claim–Evidence Extraction for Political Discourse Analysis: A Large Language Model Approach to Rodong Sinmun Editorials |
| 학술지 · 권호 | *Proceedings of the Eighth Fact Extraction and VERification Workshop (FEVER)*, Association for Computational Linguistics (ACL 2025 워크숍) |
| 연도 · 쪽수 | 2025 (July 31, 2025), pp. 1–17 (본문 1–9, 참고문헌 10, 부록 A 평가 루브릭 11–17, 부록 B 전문가 분석 예시 17) |
| DOI | 원문에 없음 |
| 투고/심사/게재 | 원문에 없음 (워크숍 개최일 2025.07.31만 표기) |
| 연구 지원 | 원문에 없음 (사사 항목 없음) |

```bibtex
@inproceedings{choi2025claimevidence,
  author    = {Choi, Gyuri and Kim, Hansaem},
  title     = {Automated Claim--Evidence Extraction for Political Discourse Analysis: A Large Language Model Approach to {Rodong Sinmun} Editorials},
  booktitle = {Proceedings of the Eighth Fact Extraction and VERification Workshop (FEVER)},
  year      = {2025},
  pages     = {1--17},
  publisher = {Association for Computational Linguistics}
}
```

## 2. 한 줄 요약
2021년 로동신문 사설 87건 전체에 CoT 프롬프트로 "핵심 주장 → 직접 인용 근거 → 정치적 의도 해석"을 뽑게 하고, 4개 LLM(GPT-o3 Mini, Claude 3.7, Gemini 2.0, EXAONE 7.8B)의 출력을 GPT-4o 기반 G-Eval로 채점했다. 점수는 대체로 5점 만점에 가깝게 나왔지만, 모델들은 수령 중심 수사를 무비판적으로 재생산하거나 해석적 환각을 만들었고, 저자들은 전문가 판단을 결합한 Hybrid Human-in-the-Loop 틀을 제안한다.

## 3. 내용 요약

**연구 질문**
1. LLM이 이념적·간접적 수사로 짜인 로동신문 사설에서 주장–근거 구조를 자동으로 추출할 수 있는가? (담화 수준 추론 능력)
2. 모델(범용 대형 모델 vs 한국어 특화 소형 모델)과 프롬프트 조건(0-shot vs 1-shot)에 따라 일관성·응답 품질·환각이 어떻게 달라지는가?
3. GPT-4o 기반 자동 평가(G-Eval)는 사람의 판단과 얼마나 일치하는가?

저자들은 이 연구를 "북한 사설의 핵심 주장과 근거를 자동 추출·평가하는 첫 프레임워크"라고 소개함 (p. 1).

### 3-1. 방법론
- **파이프라인** (Figure 1, p. 2): 사설 텍스트 → LLM 분석(CoT) → G-Eval 자동 평가(일관성 / 응답 품질 / 환각) → 모델 간 비교 → 일부 샘플 인간 평가
- **CoT 프롬프트** (pp. 2–3): Baek (2023)의 로동신문 사설 텍스트 구조 분석에서 착안함
  - 본문 설명은 3단계: (1) 핵심 주장 식별 → (2) 근거 인용 → (3) 정치적 목적 해석
  - 실제 프롬프트(Table 1)는 5단계: ① 메타데이터(제목·날짜·출처) 명시 → ② 반복 강조되는 중심 주장 식별 → ③ 사설에서 **직접 인용** 1개 이상 → ④ 정치적 목적·전략 메시지 해석 → ⑤ 고정 형식으로 요약
  - 시스템 역할: "정치 선전과 사설 담론 전문 분석가"
  - 출력 형식 고정 (Table 2): `◆ Summary of Key Themes / • Evidence / • Interpretation of Political Strategy or Message`
- **프롬프트 조건**: 0-shot과 1-shot 두 가지. 1-shot은 전문가가 쓴 예시 1개만 사용. 예시 교체나 프롬프트 민감도는 검토하지 않았고, 과적합 위험을 이유로 multi-shot은 제외함 (p. 3)
- **평가 모델(judge)**: 북한 관련 내용과 편향에 대한 파일럿 실험에서 GPT-4o가 "이해와 절제된 표현 모두에서 비교적 안정적"이어서 모든 자동 평가를 GPT-4o로 수행함 (p. 2)
- **G-Eval 평가 차원** (p. 3, 루브릭은 부록 A pp. 11–17): 모두 1–5점
  - **Coherence**: 모델이 뽑은 주장과 인용 근거 사이의 논리적 연결·추론 타당성 (Yin & Roth 2018의 구조 인식 평가 참조)
  - **Response Quality**: Accuracy(원문과의 사실 일치) / Relevance(중심 주제 반영) / Logical Consistency(응답 내부 일관성)로 세분 (Zhong et al. 2022 참조)
  - **Hallucination**: 4개 유형을 정의함 — factual insertion, emotional/ideological embellishment, causal overreach(logical leap), thematic abstraction(generalization). 단, **유형별 점수가 아니라 응답당 종합 점수 1개**를 매기고 4유형은 판단 지침으로만 씀 (p. 6). 부록 루브릭상 **5점 = 심각한 왜곡, 1점 = 환각 없음** (pp. 16–17)
- **인간 검증**: KINSS 전문가 보고서를 기준으로 무작위 일부 출력을 사람이 재검토 (p. 4). 5.4절에서는 데이터의 약 30%로 G-Eval과 인간 평가의 상관을 계산함 (p. 8)

### 3-2. 데이터셋
| 항목 | 내용 |
|---|---|
| 출처 | 로동신문 사설 (원문 한국어 그대로 수집) |
| 기간 · 규모 | **2021년 발행 사설 전체, 87건** (p. 3) |
| 주제 | 정치, 외교, 경제 등 |
| 처리 단위 | 사설 1건씩 개별 입력 (담화 수준 분석) |
| 정답(ground truth) | **국가안보전략연구원(KINSS)의 전문가 분석 보고서** — 북한 정치·군사·남북관계 전문가가 사설별로 정치 메시지와 수사 구조를 해석한 문서 (p. 4, 각주 1에 INSS 북한 자료 목록 URL). 예시는 부록 B (2021년 5월 7일자 "주체사상을 세계관·인생관으로…" 사설 분석, p. 17) |
| 전처리 · 라벨링 | 별도 전처리 서술 없음. 인간 검토는 전문가 1인 (p. 9, Limitations) |
| 공개 여부 | 데이터·프롬프트 전문·모델 출력·평가 점수 공개 여부 **원문에 없음** |

### 3-3. 실험

**설정**
- 비교 모델 4종 (Table 3, p. 4): **GPT-o3 Mini**(OpenAI), **Claude 3.7 Sonnet**(Anthropic), **Gemini 2.0 Flash**(Google), **EXAONE 7.8B**(LG AI Research). 범용 능력과 한국어 적응도로 선정. EXAONE은 규모는 작지만 한국어 정렬이 로동신문 특유의 수사 해석에 유리할 것이라는 가설로 포함함
- 조건: 0-shot / 1-shot × 4개 모델
- 평가자: GPT-4o (G-Eval, 1–5점). 별도의 비-LLM 베이스라인(규칙 기반, 키워드 기반 등)은 없음
- 온도, 디코딩 설정, 실행 횟수: 원문에 없음

**결과 ① Claim–Evidence Coherence** (Table 4, p. 4)

| 모델 | 0-shot | 1-shot |
|---|---|---|
| GPT-o3 Mini | 4.99 | **5.00** |
| Claude 3.7 | 4.99 | 4.95 |
| Gemini 2.0 | 4.95 | 4.99 |
| EXAONE 7.8B | 4.95 | 4.80 |

- Claude 3.7: 응답의 약 **4.6%**가 3점 — 근거가 주장과 직접 맞물리지 않고 설명적임 (p. 5)
- Gemini 2.0: 추상적 주제에서 약 **1.1%**가 근거 구체성 부족 (p. 5)
- EXAONE 7.8B: 약 **12.6%**에서 근거가 없거나 약하게 연결됨. 감정적·상징적 수사에 매달려 논리 분석으로 넘어가지 못함 (p. 5)

**결과 ② Response Quality** (Table 5, p. 5; 0-shot / 1-shot)

| 모델 | Accuracy | Relevance | Logical Consistency |
|---|---|---|---|
| GPT-o3 Mini | 4.94 / 4.85 | 4.98 / 4.96 | 5.00 / 5.00 |
| Claude 3.7 | 4.90 / 4.75 | 4.90 / 4.95 | 5.00 / 4.96 |
| Gemini 2.0 | 4.80 / 4.72 | 4.85 / 4.84 | 4.84 / 4.83 |
| EXAONE 7.8B | 4.31 / **3.86** | 4.84 / 4.20 | 4.82 / 4.33 |

- 표를 보면 대부분의 모델에서 1-shot이 0-shot보다 오히려 **낮음**. 그런데 본문은 "1-shot에서 전문가 분석과의 정렬이 좋아졌다"고 서술함 (p. 4, p. 9) → 아래 "내가 본 한계" 참조
- EXAONE 오류 사례 (Table 6, p. 6): 김정일 총비서 추대 기념 사설을 김정은의 당 건설 업적 중심으로 해석해 Accuracy 1점. 상징적 계승과 주제적 중심을 구분하지 못함
- 인간 분석가는 이 사설이 제8차 당대회에서의 김정은 직함 변경을 반영해 전략적으로 짜였다고 추론했지만, LLM은 사설 본문에만 머묾 (p. 6)

**결과 ③ Hallucination** (Table 7, p. 7; 높을수록 환각이 심함)

| 모델 | 0-shot | 1-shot |
|---|---|---|
| GPT-o3 Mini | 4.52 | **4.21** (최저) |
| Claude 3.7 | 4.75 | 4.52 |
| Gemini 2.0 | 4.92 | 4.92 (최고) |
| EXAONE 7.8B | 4.47 | 4.57 (shot 추가 후 상승) |

- Claude 3.7: 수령 중심·동원 주제 사설에서 감정적 수사를 무비판적으로 재생산 (p. 7)
- Gemini 2.0: 2021.02.14 사설(8기 2차 전원회의 정신으로 5개년 계획 첫해 과업 관철)을 "당 결정에 대한 무조건 복종"을 강조하는 "전형적 북한 선전"으로 재프레이밍 → 정책·실행 지향 메시지를 이념적 복종으로 왜곡 (Table 8, p. 7)
- EXAONE 7.8B: "위대한", "절대적", "역사적" 같은 표현에 과잉 반응. 원문에 없는 "역사적"을 김정은 연설에 붙여 환각 점수 5점 (Table 9, p. 7)

**결과 ④ G-Eval과 인간 평가의 상관** (Table 10, p. 8; 데이터 약 30%)

| 차원 | 0-shot | 1-shot |
|---|---|---|
| Accuracy | 0.64 | 0.78 |
| Relevance | 0.59 | 0.75 |
| Logical Consistency | 0.73 | 0.64 |
| Coherence | 0.55 | 0.60 |
| Hallucination | 0.65 | 0.77 |

- 대부분 p < 0.05로 유의하다고 보고함. 본문은 "Pearson", 표 제목은 "Spearman"이라고 적어 **상관계수 종류가 불일치**함 (p. 8)
- Logical Consistency만 1-shot에서 상관이 떨어짐 → 예시에 과의존해 추론을 단순화했을 가능성 (p. 8)
- GPT-4o는 점수가 높은 쪽으로 쏠림. 명백한 논리 불일치가 있는 응답에도 coherence 4점 이상을 줌 → 사람보다 관대한 채점 (p. 8)

### 3-4. 결론
- LLM은 CoT 구조화로 "그럴듯한 수준"의 주장–근거 연결을 만들 수 있고, 1-shot에서 전문가 분석과의 정렬이 높아진다고 주장함 (p. 9)
- 그러나 모델은 정치적 상징·감정 수사를 비판 없이 재생산하거나 과잉 해석함. 정치 담론에서 환각은 단순한 사실 오류가 아니라 "수사 형식이 의도된 의미를 왜곡하는 고차원 생성 실패"라고 규정함 (p. 8)
- 모델별로 GPT-o3 Mini가 가장 안정적이고, EXAONE 7.8B와 Gemini 2.0은 정치적 상징에 과잉 반응함 (p. 8). 다만 EXAONE의 한국어 수사 민감도는 도메인 특화 튜닝의 가능성으로 해석함 (p. 6)
- G-Eval은 인간 평가와 "상당한" 상관을 보이지만 미묘한 맥락 오류와 수사 왜곡은 잡지 못하므로, 자동 채점을 **후처리가 아니라 보완**하는 Hybrid Human-in-the-Loop 틀이 필요하다고 결론 냄 (pp. 8–9)

## 4. 해당 연구의 한계점

**저자가 밝힌 한계** (p. 9, Limitations)
- 생성 응답의 평가를 **전문가 1인**이 수행해 주관적 편향 가능성이 있음. 환각 오류 분류는 GPT-4o의 자동 판단에만 의존했고, 평가자 간 일치도나 명시적 주석 지침이 없음
- 5단계 프롬프트의 각 단계 기여를 보는 ablation이 없음. 1-shot은 예시 1개뿐이고 예시 선택에 대한 민감도를 체계적으로 평가하지 않음
- 환각 점수는 4유형을 합친 종합 점수라서 유형별 빈도와 영향을 분석하지 못함
- 특정 시기(2021년) 로동신문 사설만 다뤄 시기·장르 일반화가 검증되지 않음

**내가 본 한계**
- **점수 천장 효과와 변별력 부족**: Coherence가 4.80–5.00, Logical Consistency가 4.33–5.00에 몰려 있어 모델 간 차이가 0.01–0.05 수준임. 분산·신뢰구간·유의성 검정 없이 "GPT-o3 Mini가 가장 우수"하다고 서술함. 저자 스스로 GPT-4o가 관대하게 채점한다고 인정했으므로(p. 8) 절대 점수 해석은 매우 조심해야 함
- **환각 척도 방향과 수치가 서로 맞지 않음**: 부록 루브릭은 5점 = "심각한 왜곡", 4점 = "여러 문장에 이념적 과장"인데(p. 16), 모든 모델의 평균이 4.21–4.92임. 루브릭대로라면 모든 모델이 거의 모든 사설에서 심각하게 환각했다는 뜻인데, 같은 출력에 대해 GPT-o3 Mini·Claude·Gemini의 Accuracy는 4.72–4.94로 "원문 사실에 거의 완전히 충실"하다고 나옴. 또 Table 9에서 '역사적' 한 단어 삽입에 5점을 줬는데 루브릭상으로는 2–3점 수준의 수사 첨가에 가까움. 척도가 실제로 어떻게 적용됐는지 원문만으로는 알 수 없음
- **본문 주장과 표의 불일치**: Response Quality(Table 5)는 대부분 1-shot에서 하락했고(EXAONE Accuracy 4.31→3.86), Coherence도 Claude·EXAONE은 하락함. 그런데 초록·결론은 "1-shot에서 전문가 분석과의 정렬이 현저히 향상"이라고 씀. 근거로 볼 만한 것은 Table 10의 상관 상승뿐인데, 이는 **평가자(GPT-4o)와 사람의 일치도**이지 **모델 출력과 전문가 분석의 일치도**가 아님. 두 개념이 섞여 있음
- **Ground truth가 실제 채점에 쓰였는지 불분명**: KINSS 보고서가 G-Eval의 참조(reference)로 들어갔는지, 인간 검토에서만 쓰였는지 명시되지 않음. 참조 없이 원문만 보고 채점했다면 "전문가 분석과의 정렬"이라는 표현은 과장임
- **평가자와 피평가자 계열 중복**: 평가자가 GPT-4o이고 최고 성적 모델이 같은 OpenAI 계열 GPT-o3 Mini임. LLM judge의 self-preference 편향(저자가 인용한 Stureborg et al. 2024의 문제 제기와 같은 맥락)을 통제하지 않음. 다른 계열 judge나 다중 judge 비교가 필요함
- **'환각'과 '해석'의 경계**: Gemini가 사설을 "전형적 북한 선전"으로 프레이밍한 것을 환각으로 처리했는데(Table 8), 이는 원문 충실성 관점에서는 왜곡이지만 분석가 관점에서는 정당한 외부 해석일 수도 있음. 프롬프트 자체가 "정치 선전 전문가"로서 "정치적 의도를 해석"하라고 요구하므로(Table 1), 과제 정의와 환각 정의가 충돌함
- **재현성 부족**: 모델 버전 문자열, API 호출 날짜, 온도·디코딩 설정, 1-shot 예시 원문, 87건 사설 목록, 모델 출력이 공개되지 않음. 참고문헌의 모델 표기도 부정확함(Claude 3.7을 Claude 3.5 Sonnet 발표문으로, EXAONE 7.8B를 EXAONE Deep 논문으로 인용, o3-mini 발표일을 2024.04.17로 표기)
- **세부 오류**: Table 6의 사설 제목이 "김정일 총비서 추대 24주년 (Oct 8, 1994)"으로 되어 있는데, 2021년 사설에서 24주년이면 기준 연도는 1997년이어야 함. 사소하지만 북한 텍스트를 다루는 논문에서 날짜 정확성은 중요함
- **베이스라인 부재**: 규칙·키워드 기반 추출, 단순 요약(리드 문장) 같은 하한선이 없어 CoT 구조화가 실제로 얼마나 기여하는지 알 수 없음. 저자도 ablation 부재를 인정함
- **북한어 특성 미분석**: 문화어 어휘·두음법칙 등 남북 언어 차이가 모델 성능에 어떤 영향을 주는지는 다루지 않음. EXAONE의 부진을 "한국어 특화"와 연결하지만 남한어 특화가 북한어에 그대로 이전되는지는 검증하지 않음

## 5. 추가로 읽을 유사 논문

📎 = 본 논문의 참고문헌에서 가져옴 (원문 미확인)

| 논문 / 자료 | 왜 읽나 | 확인 |
|---|---|---|
| Baek, Seungjoo (2023). Analyzing the textual organization of North Korean Rodong Sinmun editorials. *The Journal of Language & Literature*, 94, 85–118. 10.15565/jll.2023.3.93.85 | 본 논문 CoT 프롬프트의 이론적 원천. 로동신문 사설이 논증보다 설득 중심으로 짜인다는 텍스트 구조 분석. 주장–근거 스키마를 설계할 때 기준이 됨 | 📎 |
| Kim, Jeong-ho & Cho, Yunyoung (2022). North Korea's Political-Economic Changes Under the Five-Year Economic Development Plan: Kim Jong-Un Regime's rising ruling tasks and challenges on the 「Rodong Newspaper」. *Journal of Peace Studies*, 23(1), 155–178 | 같은 시기(5개년 계획, 8차 당대회) 로동신문을 질적으로 분석한 연구. LLM 출력과 비교할 전문가 해석의 또 다른 기준 | 📎 |
| Le Mens, G. & Gallego, A. (2025). Positioning Political Texts with Large Language Models by Asking and Averaging. *Political Analysis*, 1–9. doi:10.1017/pan.2024.29 | LLM으로 정치 텍스트의 이념 위치를 추정하는 방법. 북한 매체의 대남·대미 입장 척도화에 응용 가능 | 📎 |
| Stureborg, R., Alikaniotis, D., & Suhara, Y. (2024). Large language models are inconsistent and biased evaluators. arXiv:2405.01724 | 본 논문의 G-Eval 결과(천장 효과, 관대한 채점)를 비판적으로 읽기 위한 필수 문헌 | 📎 |
| Gao, Q. & Feng, D. W. (2025). Deploying large language models for discourse studies: An exploration of automated analysis of media attitudes. *PLOS ONE*, 20(1), e0313932. doi:10.1371/journal.pone.0313932 | 언론 텍스트의 태도(stance) 자동 분석. 담론 연구에서 LLM을 쓰는 방법론 비교 기준 | 📎 |
| Yang, K. et al. (2024). Unpacking Political Bias in Large Language Models: Insights Across Topic Polarization. arXiv:2412.16746 | 모델 출신·규모·학습 시점에 따른 정치 편향. 북한 텍스트에서 모델별 프레이밍 차이(Gemini의 '선전' 프레이밍 등)를 설명할 단서 | 📎 |
| Kronlund-Drouault, P. (2024). Propaganda is all you need. arXiv:2410.01810 | 정렬(alignment)이 지배적 이데올로기를 반영할 수 있다는 주장. 북한 텍스트 해석의 구조적 편향 논의에 연결 | 📎 |
| [004] Cho, Cho & Seo (2025). Hermit Kingdom Through the Lens of Multiple Perspectives (COLING 2025) → [정리 문서](004_llm-hallucination-north-korea.md) | 같은 배치의 자매 논문. 003이 "주어진 북한 텍스트의 해석 환각"이라면 004는 "북한에 관한 사실 지식 환각". 두 축을 합쳐야 북한 연구용 LLM 신뢰도를 말할 수 있음 | 아카이브 |
| [001] 황수영 (2026). 한국 외교부의 대중국 공식 담론 분석 → [정리 문서](001_mofa-china-discourse-llm-coding.md) | LLM 보조 코딩 방법론의 국내 사례. 001의 코드북 방식과 003의 CoT+G-Eval 방식을 비교해 볼 것 | 아카이브 |

## 6. NLP 측면 연구 아이디어

### 아이디어 1. 로동신문 주장–근거 골드 데이터셋과 다중 평가자 신뢰도 감사
- **문제 정의**: 003은 전문가 1인, GPT-4o 단일 judge, 천장에 붙은 점수 때문에 "LLM이 북한 사설을 얼마나 잘 분석하는가"에 대한 답을 사실상 주지 못함. 사람이 만든 **span 수준 골드 라벨**을 만들어 자동 지표 자체를 검증함
- **데이터**: 로동신문 사설 (003과 같은 2021년 87건으로 시작해 다른 연도로 확장). 각 사설에 대해 (a) 중심 주장 문장, (b) 근거 문장 span, (c) 주장–근거 관계 유형(인용·업적 나열·지시·대조 등)을 북한학 전공자 2–3명이 주석. KINSS 보고서는 해석 층위의 참고 자료로만 사용
- **방법**: 생성형 요약 평가 대신 **추출형 과제**로 재정의 → span F1, 관계 분류 정확도로 측정 가능하게 만듦. LLM judge는 GPT-4o 외에 Claude·Gemini·오픈 모델(EXAONE, Qwen 등)을 함께 써서 judge 간 편향과 self-preference를 측정
- **평가**: 인간 간 일치도(Krippendorff's α)를 먼저 보고 → judge–인간 일치도를 그 상한과 비교. 점수 분포(천장 효과)와 judge별 관대함을 보정한 후 모델 순위가 바뀌는지 확인
- **기대 기여**: 북한 텍스트 B 연구 전체에 쓸 수 있는 공개 평가 자원. README의 연구 공백 1(재현 가능한 데이터셋 부재)과 2(LLM 코딩 신뢰도 보고 부족)를 동시에 메움

### 아이디어 2. "해석"과 "환각"의 분리: 북한 사설 요약에서의 프레이밍 주입 측정
- **문제 정의**: 003에서 Gemini의 "전형적 선전" 프레이밍과 EXAONE의 '역사적' 삽입은 같은 '환각'으로 처리됨. 그러나 전자는 **외부 관점의 평가 프레임 주입**, 후자는 **원문 수사의 증폭**으로 방향이 반대임. 이 두 방향을 분리해 측정함
- **데이터**: 로동신문 사설 + 모델 요약. 문장마다 (i) 원문에 근거 있음, (ii) 원문 수사 증폭(찬양어 추가), (iii) 외부 평가 프레임 주입("선전", "독재", "형식적" 등), (iv) 사실 삽입으로 라벨링
- **방법**: NLI 기반 근거성 검사(요약 문장 ↔ 원문)로 (i)/(iv)를 거르고, 평가어 사전 + LLM 분류로 (ii)/(iii)을 구분. 프롬프트 역할("선전 분석가" vs "중립 요약자" vs "북한 기자")을 바꿔 가며 프레이밍이 얼마나 이동하는지 측정
- **평가**: 모델·언어·역할별 (ii)/(iii) 비율. 사람 주석과의 일치도
- **기대 기여**: 북한 연구자가 LLM 요약을 쓸 때 "원문이 무엇을 말했는가"와 "모델이 어떻게 평가했는가"를 구분할 수 있는 진단 도구. 004의 다국어 편향 문제와 연결됨

### 아이디어 3. 로동신문 사설의 시계열 주장 구조 변화 추적 (2012–현재)
- **문제 정의**: 003은 2021년 한 해만 다룸. 주장–근거 추출을 김정은 시기 전체로 넓혀 **주장 유형과 근거 동원 방식의 변화**(예: 김일성·김정일 고전 인용 vs 김정은 연설 인용, 경제 성과 vs 사상 강조)를 정량화함
- **데이터**: 로동신문 사설 다년치 (공개 아카이브에서 수집 가능한 범위). 8차 당대회(2021), '김정은주의' 언급(003 각주 2) 전후 같은 이벤트를 구분점으로 설정
- **방법**: 아이디어 1의 골드 데이터로 검증한 추출기를 적용 → 근거 출처(누구의 말을 인용하는가), 주장 주제, 수사 강도 지표를 연도별로 집계. 변화점 탐지(change-point detection)
- **평가**: 표본 연도에 대한 인간 검증, 기존 질적 연구(Kim & Cho 2022 등)의 시기 구분과 대조
- **기대 기여**: 질적 연구가 직관적으로 말해 온 "담론 전환"을 재현 가능한 지표로 제시. 001-#2(대외 역할 부여 추출)와 결합하면 대내 사설과 대외 메시지를 함께 볼 수 있음

## 7. 메모 · 인용하고 싶은 문장
- "hallucination in political discourse is not merely a factual error but a higher-order generation failure where rhetorical form distorts intended meaning." (p. 8) — 정치 담론 환각의 정의로 인용할 만함
- "GPT-4o tends to make more lenient judgments than human evaluators." (p. 8) — LLM-as-a-judge를 북한 텍스트에 쓸 때의 경고
- Hybrid HITL 제안: "human interpretation complements, rather than simply post-processes, automated outputs" (p. 8)
- 인간 분석가는 사설을 8차 당대회 직함 변경과 연결해 읽었지만 LLM은 본문에만 머묾 (p. 6) → **맥락 지식(시점·이벤트) 주입**이 핵심 개선 방향. RAG로 당대회·전원회의 일정을 주는 실험을 고려할 것
- 확인할 것: 저자 측 데이터·프롬프트 공개 여부, ACL Anthology ID와 DOI (원문에 없음)
