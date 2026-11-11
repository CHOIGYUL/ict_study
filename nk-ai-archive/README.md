# 북한 × AI 연구 아카이브

북한 연구 가운데 다음 두 갈래에 해당하는 논문을 모아 정리하는 아카이브입니다.

- **A. 북한의 AI·ICT 기술**: 북한이 AI·사이버·정보통신 기술을 어떻게 개발하고 쓰는지 다룬 연구
- **B. AI·NLP로 북한을 분석한 연구**: 텍스트마이닝, LLM, 기계번역 등으로 북한(또는 북한 관련 담론)을 분석한 연구
- **C. 방법론 참고**: 북한을 직접 다루지는 않지만 B 연구에 그대로 가져다 쓸 수 있는 방법론 논문

모든 정리 문서는 같은 틀을 따릅니다 ([TEMPLATE.md](TEMPLATE.md)).
**논문 제목 → 서지 정보 → 본문 요약 → 한계점 → 추가로 읽을 유사 논문 → NLP 연구 아이디어**

## 정리된 논문

| # | 제목 | 저자 · 연도 | 학술지 | 분류 | NLP 아이디어 |
|---|---|---|---|---|---|
| [001](papers/001_mofa-china-discourse-llm-coding.md) | 한국 외교부의 대중국 공식 담론 분석(2022~2025): LLM 보조 코딩을 활용한 정책 태도와 역할 부여 | 황수영 (2026) | 한중관계연구 12(3) | C (+B) | 대북 담론 역할 부여 코딩 / 북한 매체의 대외 역할 추출 / 북한 텍스트에서 LLM 코더 신뢰도 감사 |
| [002](papers/002_nk-cyber-ai-nexus-digital-panopticon.md) | 북한의 사이버-AI 넥서스 전략과 디지털 파놉티콘: 보편적 기술의 의도적 전유와 사회적 재구성 | 이종진 (2026) | 통일과 평화 18(2) | A | 말투 판별 실현 가능성 감사 / 북한 AI 문헌 과학계량 / 법령 legal NLP / 통제·우회 이벤트 타임라인 / 검열 어휘 사전 |
| [003](papers/003_rodong-sinmun-claim-evidence-llm.md) | Automated Claim–Evidence Extraction for Political Discourse Analysis: A LLM Approach to Rodong Sinmun Editorials | Choi & Kim (2025) | FEVER 2025 (ACL WS) | B | 주장–근거 골드셋과 평가자 신뢰도 감사 / 해석과 환각 분리 / 사설 주장 구조의 시계열 변화 |
| [004](papers/004_llm-hallucination-north-korea.md) | Hermit Kingdom Through the Lens of Multiple Perspectives: A Case Study of LLM Hallucination on North Korea | Cho, Cho & Seo (2025) | COLING 2025 | B | NK-TruthfulQA 다국어 벤치마크 / 출처 계층별 RAG / 언어 간 프레이밍 비교 |
| [005](papers/005_north-korean-mt-through-south-korean.md) | Learning How to Translate North Korean through South Korean | Kim, Moon, Okazaki & Komachi (2022) | LREC 2022 | B | 북한어→남한어 정규화 벤치마크 / 2021년 이후 MT 평가셋 / 대내용·대외용 메시지 정렬 비교 |
| [006](papers/006_scalable-qualitative-coding-llm.md) | Scalable Qualitative Coding with LLMs (프리프린트, 저널판: Scaling hermeneutics) | Dunivin (2024/2025) | arXiv / EPJ Data Science | C | 코드 문구·북한어 표기 민감도 감사 / 북한 매체로 설계 재현 / 선택적 사람 검토의 비용–정확도 |
| [007](papers/007_nk-cyber-ai-national-strategy.md) | 김정은 시대 북한의 사이버-AI 국가전략: 전략적 인식, 역량 강화, 대외협력 | 윤훈희 (2025) | 현대북한연구 28(3) | A | 북한 AI 학술지의 기술 도입 시차 / 로동신문 AI 담론 프레임 / AI 응용 사례 이벤트 DB |
| [008](papers/008_rodong-sinmun-climate-discourse-textmining.md) | 김정은 시기 기후변화 담론 변화 분석: 로동신문에 나타난 성과 정당화와 한계 | 류경아·홍건식 (2024) | 현대북한연구 27(1) | B | 북한어 전처리 민감도 감사 / 기후 피해 책임 귀속 추출 / 북한식 신조어 탐지 |
| [009](papers/009_nk-economic-policy-lda-topic-modeling.md) | 토픽모델링을 통한 북한의 경제정책 동향 분석 | 강경화 (2020) | 스마트미디어저널 9(4) | B | 토픽 모델 재현·강건성 감사 / 북한어 전처리 영향 / 제목 vs 본문 검증 |
| [010](papers/010_project-reveal-nk-digital-control.md) | Project Reveal: New Research into North Korea's Digital Control System (보고서) | Williams & Schiess (2022) | Lumen | A | 주장 출처 계보 추적 / 단말 공식 언어 자원 목록화 / 공식 보도 이벤트 타임라인 |

다음에 읽을 후보는 [reading_list.md](reading_list.md)에 모아 두었습니다.

## 지금까지 정리한 내용에서 보이는 연구 공백

1. **재현 가능한 북한 텍스트 데이터셋이 거의 없음**: 정리한 논문 대부분이 데이터나 코드북을 공개하지 않았음 (005는 신청서를 통해서만 제공). 북한어 NLP 자원도 Kim et al. (LREC 2022)의 번역 데이터 정도임
2. **LLM 보조 코딩의 신뢰도 보고가 약함**: 001은 일치도 지표 없이 연구자 1인이 최종 판정했음. 북한 텍스트에서는 LLM의 편향과 환각 문제(Cho et al., COLING 2025)가 더해짐
3. **북한 AI 기술에 대한 주장이 2차 자료에 몰려 있음**: 002의 'AI 말투 식별 90%'는 38 North 기사 하나에 기대고 있음. 북한 AI 논문 원문을 분석하거나 공개 데이터로 실현 가능성을 검증하는 연구가 비어 있음
4. **2차 인용 과정에서 사실이 바뀜**: 002가 Project Reveal을 인용한 내용 일부(비둘기·참매의 성격, 워터마크 용도, '10–30%' 수치)가 원문과 다름 (→ 010, 002 한계 항목). 북한 디지털 통제 연구는 1차 자료까지 거슬러 확인할 필요가 있음
5. **북한 텍스트 분석은 아직 초기 기법에 머묾**: 008(워드클라우드·파이계수 네트워크), 009(제목만으로 LDA)처럼 빈도 기반 분석이 주류이고, 북한어 전처리 방법이나 데이터가 공개되지 않음. 005·003·004 같은 NLP 쪽 연구와 아직 잘 이어지지 않음

### 아이디어 우선순위 (초안)
| 순위 | 아이디어 | 출처 | 왜 먼저 하나 |
|---|---|---|---|
| 1 | 로동신문·조선중앙통신의 대외 역할 부여 추출 (남북 공통 코드북) | 001-#2 | 데이터를 공개 자료로 바로 구할 수 있고, Choi & Kim (2025)이라는 비교 기준이 있음 |
| 2 | 북한 텍스트에서 LLM 코더 신뢰도 감사 | 001-#3 | 다른 모든 B 연구의 전제가 되는 방법론 기여 |
| 3 | 남북 언어변이 판별의 실현 가능성 감사 (오탐 중심, 이중 용도 주의) | 002-#1 | 북한 AI 검열 주장을 정면으로 검증하는 연구. 다만 연구윤리 설계가 먼저 필요함 |

## 운영 규칙
- 파일 이름: `papers/NNN_영문-슬러그.md`
- 서지 확인 표시: ✅ 웹에서 확인 · 📎 다른 논문 참고문헌에서 가져옴 · ❓ 일부 확인 필요
- 원문 PDF는 저작권 문제로 저장소에 올리지 않음. DOI 링크만 기록함
- 감시·검열 관련 아이디어에는 이중 용도(dual-use) 주의 문구를 붙임
