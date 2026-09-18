# 한국어 챗봇 응답 생성 프로젝트 결과보고서

## 1. 목표
- 토큰화, 임베딩, Transformers 사전학습 모델을 활용한 파이프라인 설계
- 전이학습(transfer learning) 기반 Q→A 응답 생성 모델 학습 및 평가

## 2. 데이터
- 출처: `dataset/ChatbotData.csv` (Q, A, 감정 label 0/1/2)
- 규모: 원본 N건 → 정제 후 N'건 (`src/data_prep.py` 실행 로그 참고)
- 전처리: 공백 정규화, 중복 제거, train/val/test 분할(비율은 `config.yaml` 참고)
- (TODO) 길이 분포, label 분포 통계/그래프 삽입

## 3. 토큰화 및 임베딩
- 사용 토크나이저: `config.yaml`의 `model.name` 기입
- (TODO) `src/tokenization_demo.py` 실행 결과: 서브워드 분해 예시, vocab size, 임베딩 차원
- (TODO) 임베딩 코사인 유사도 결과 및 해석

## 4. 모델 선택 근거 (전이학습)
- 후보로 검토한 모델과 최종 선택: (TODO)
- 선택 근거: 과제 적합성(생성 vs 분류), 한국어 사전학습 여부, 모델 크기/학습 가능 시간, 라이선스 등 기준으로 작성
- 전이학습 적용 방식: Hugging Face `from_pretrained`로 사전학습 가중치를 불러와 Q/A 데이터로 파인튜닝 (`src/model.py`, `src/train.py`)

## 5. 파이프라인 설계
- 데이터 전처리(`src/data_prep.py`) → 토큰화(`src/dataset.py`) → 모델 로드(`src/model.py`)
  → 학습(`src/train.py`) → 생성(`src/generate.py`) → 평가(`src/evaluate.py`) → 오류분석(`src/error_analysis.py`)
- (TODO) 파이프라인 다이어그램 삽입

## 6. 학습
- 하이퍼파라미터: `config.yaml`의 `train` 섹션 값 기입 (epoch, batch size, lr 등)
- (TODO) 학습 loss curve, 학습 시간, 사용 하드웨어(GPU/CPU)

## 7. 평가 (생성 과제 지표)
| 버전 | BLEU | ROUGE-L | Distinct-1 | Distinct-2 | 빈 응답 비율 |
|---|---|---|---|---|---|
| baseline (파인튜닝 전) | | | | | |
| 1차 파인튜닝 | | | | | |
| 개선판 | | | | | |

(위 표는 `outputs/metrics_<tag>.json` 값을 옮겨 채운다. `python -m src.evaluate --model_path <원본모델> --tag baseline` 로 파인튜닝 전 베이스라인도 함께 측정)

## 8. 오류 분석 (baseline → 개선 과정 기록)
- `report/error_log.md`에 tag별로 누적 기록됨 (baseline, finetuned, epoch2, ... )
- (TODO) 오류 유형 분포 변화 요약: 어떤 오류가 줄었고 어떤 오류가 남았는지
- (TODO) 대표 실패 사례 3~5개와 원인 분석, 개선 시도(하이퍼파라미터/데이터/디코딩 전략 변경 등)

## 9. 결론 및 한계
- (TODO) 최종 성능 요약, 남은 한계, 향후 개선 방향
