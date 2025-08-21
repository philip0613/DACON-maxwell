# -*- coding: utf-8 -*-
2025 전력사용량 예측 AI 경진대회 — 최종 파이프라인 (주간프로파일+잔차학습+스태킹+TTA, submission.csv 자동저장)

핵심:
- 주간 프로파일(건물×week-hour 중앙값)로 1차 정규화 → 모델은 잔차(y / wk_prof)를 학습 (scaled2)
- 모델 가족: LightGBM / XGBoost / CatBoost(MAE) / HistGB / (옵션)MLP — 가용한 것만 자동 사용
- 가족별 배깅(n_models 시드) → 검증셋에서 메타 스태킹(Ridge, scaled2 space)
- 검증 기반 잔차 바이어스맵(건물×week-hour, scaled2) + 건물별 Huber 캘리브레이션(원공간)
- Test-Time Augmentation(TTA)으로 날씨 미세 흔들기 후 평균
- 오토리그레시브(−168h 나이브)와 동적 블렌딩(건물별) + 상한 클리핑
- test에 없는 칼럼(일조/일사) 제거, 경로 자동 탐색(./open, ./venv/open 등)
- PyCharm/Colab 호환(parse_known_args)
- 출력: ./submissions/submission_*.csv + ./submissions/submission.csv (고정 이름)
