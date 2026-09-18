# 작업 계획 (한국어 챗봇 Q→A 응답 생성)

작업 방식: **작업순서설정 → 노트북 작성 → 사용자 실행/결과 확인 → 다음 작업 진행**
(노트북은 요청받은 부분만 작성/수정하고, 실행/결과 확인은 사용자가 직접 진행)

커널: Jupyter에서 노트북 열 때 우측 상단 커널을 **`nlp_project (.venv)`** 로 선택 (프로젝트 전용 `.venv`, GPU/CUDA 포함 패키지 설치 완료).

## 완료

- [x] `lecture/NLP_수업정리.ipynb` 검토, 아키텍처 확정 (베이스라인: 직접구현 Transformer seq2seq / 전이학습: KoBART·KE-T5)
- [x] 개발 환경 구성 (`.venv`, Python 3.11, torch+CUDA 12.8, transformers/gensim/konlpy 등)
- [x] `notebooks/01_data_eda.ipynb`: 데이터 로드, 결측치/중복 확인(중복 행 직접 표시 포함), label·길이 분포, percentile/max_length 설명, 정제, train/val/test 분할·저장 — **사용자 실행/피드백 대기 중**

## 다음 단계 (순서대로, 매 단계 사용자 확인 후 진행)

1. **`01_data_eda.ipynb` 실행 결과 확인** — 사용자가 직접 실행 후 이상 없는지 회신
2. **`02_tokenization_embedding.ipynb`** (범위는 실행 전 별도 확정)
   - 어절/형태소(Okt)/자모 단위 토큰화 비교
   - SentencePiece(BPE) 직접 학습 (ChatbotData 기반) vs KoBART 사전학습 토크나이저 비교 → 토큰화 방식 선택 근거 정리
   - gensim Word2Vec 임베딩 학습 → 코사인 유사도로 유사 단어 확인, 임베딩 과정 step-by-step 정리
3. **`03_baseline_transformer.ipynb`**: 수업 11번 섹션 구조 기반 직접구현 인코더-디코더 Transformer, ChatbotData로 처음부터 학습 (베이스라인)
4. **`04_finetune_pretrained.ipynb`**: KoBART(+비교용 KE-T5) 사전학습 가중치 로드 → Q→A 파인튜닝 (전이학습, 고도화 모델)
5. **`05_decoding_strategy.ipynb`**: greedy / beam search / top-k·top-p 샘플링 디코딩 전략 비교
6. **`06_hparam_comparison.ipynb`**: learning rate/batch size/epoch 등 하이퍼파라미터 비교 실험, KoBART vs KE-T5 비교
7. **`07_evaluation.ipynb`**: BLEU/ROUGE-L/Distinct-n/빈 응답 비율로 baseline vs 파인튜닝 vs 개선판 비교
8. **`08_error_analysis.ipynb`**: 오류 유형 분류, 대표 실패 사례 분석, 개선 전/후 비교
9. **보고서 작성** (`report/REPORT_TEMPLATE.md` 기반, .pdf 변환)
10. **발표자료 작성** (`slides/SLIDES_OUTLINE.md` 기반)

## 나중에 추가로 확인할 사항

- 1번 노트북

1.  Q-A완전 중복행이 LABEL은 다른 행임. 과연 진짜 삭제를 해도 될까?

각 단계 시작 전, 해당 노트북에 정확히 무엇을 넣을지 먼저 여쭙고 확정한 뒤 작성합니다.
