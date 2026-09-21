# 작업 계획 (한국어 챗봇 Q→A 응답 생성)

작업 방식: **작업순서설정 → 노트북 작성 → 실행/결과 확인 → 다음 작업 진행**

커널: Jupyter/VS Code에서 노트북을 열 때 인터프리터를 프로젝트 전용 `.venv`(`.venv\Scripts\python.exe`)로 선택한다. 등록된 커널 이름은 `nlp-project-venv`(표시명 `Python (nlp-project .venv)`).

> **마지막 갱신**: 2026-09-19. 모든 수치는 이 날짜의 실행 결과 기준이며 노트북 01~08 출력과 일치한다.

## 완료

- [X] `lecture/NLP_수업정리.ipynb` 검토, 아키텍처 확정 (베이스라인: 직접구현 Transformer seq2seq / 전이학습: KoBART·KE-T5)
- [X] 개발 환경 구성 (`.venv`, Python 3.11, torch+CUDA 12.8, transformers/gensim/konlpy 등, JDK 17)
- [X] `01_data_eda.ipynb` — 정제 11,750건, 8:1:1 분할(9,400/1,175/1,175)
- [X] `02_tokenization_embedding.ipynb` — 토큰화 4단위 비교, SentencePiece BPE(vocab 8,000), Word2Vec(6,048단어)
- [X] `03_baseline_transformer.ipynb` — 인코더-디코더 Transformer(6.0M), 30 epoch, val_loss 6.3724, 반복 붕괴 관찰
- [X] `04_finetune_pretrained.ipynb` — 베이스라인 재실험(117ep, 5.7957) + KoBART 파인튜닝(7ep, 2.7424)
- [X] `05_decoding_strategy.ipynb` — greedy/beam/top-k·top-p 비교
- [X] `06_hparam_comparison.ipynb` — 임베딩 초기화·lr·스케줄 비교, KE-T5 비교
- [X] `07_evaluation.ipynb` — 모델 9종 × test 1,175건 정량 평가 (BLEU/ROUGE-L/Distinct-n/빈 응답률)
- [X] `08_error_analysis.ipynb` — 오류 유형 분류, 개선 추적, 대표 실패 사례, **EOS 결함 규명**
- [X] **EOS 누락 결함 수정 및 전체 재실행** (04→05→06→07→08) — 상세는 `PROJECT_FLOW.md` 4.5절
- [X] 결과보고서 작성 (`report/결과보고서.pdf`, 27페이지)
- [X] 진행 로그 (`PROGRESS_LOG.pdf` ~09-18, `PROGRESS_LOG_2026-09-19.pdf`)

## 남은 작업

1. **발표자료 작성** (`slides/SLIDES_OUTLINE.md` 기반)
   - 보고서와 동일한 순서를 따르되, 핵심 스토리는 "전이학습 검증 → 예상 밖 결과 → 원인 추적 → 결함 수정 → 결론 반전"으로 구성하면 전달력이 높다.

## 향후 개선 과제 (우선순위 순)

`08_error_analysis.ipynb` 7절 및 보고서 10.3절과 동일하다.

| 순위 | 할 일                                            | 근거                                                                                              |
| ---- | ------------------------------------------------ | ------------------------------------------------------------------------------------------------- |
| 1    | 사람 평가 또는 의미 기반 지표(BERTScore 등) 추가 | KoBART의 '표면 불일치' 10건 중 8건이 실제로는 타당한 응답 — 현재 지표가 성능을 과소평가하고 있다 |
| 2    | 베이스라인의 "문맥 무시" 오류 개선               | 베이스라인 실패의 주종(수동 판정 10건 중 6건)                                                     |
| 3    | KoBART 학습 예산 확대 후 B0/B1/B2 재비교         | B1/B2가 7 epoch 예산을 다 쓰고도 수렴 전이었음                                                    |
| 4    | label 1·2 성능 보강 (가중 샘플링 등)            | label 0 대비 BLEU가 약 3분의 1 수준 (13.22 vs 3.55/3.64)                                          |
| 5    | KE-T5 학습 예산 확대 또는 비교군에서 제외        | 7 epoch에서 수렴 실패(모드 붕괴), "나쁘다"가 아니라 "판정 불가"가 정확                            |

## 미해결 항목

1. **(Q, A)는 같은데 label만 다른 행의 처리**
   - 01번에서 (Q, A) 완전 중복 73건을 제거했는데, 그중 일부는 label만 다른 행이었다 (예: "결혼이나 하지 왜 자꾸 나한테 화 내냐구!" → "힘들겠네요."가 label 0과 1로 각각 존재).
   - 현재는 label을 학습에 쓰지 않고 평가 시 분해 기준으로만 사용하므로 제거해도 무방하다고 판단했다.
   - 다만 label을 조건으로 주는 모델로 확장한다면, 이 행들은 "같은 Q-A가 두 감정 맥락에서 모두 성립한다"는 정보이므로 재검토가 필요하다.

## 참고 문서

- 설계 근거·전체 흐름: `PROJECT_FLOW.md`
- 최종 결과: `report/결과보고서.pdf`
- 지표 원본 데이터: `results/` (EOS 수정 전 결과는 `results/before_eos_fix/`에 보존)
