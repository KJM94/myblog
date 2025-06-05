---
title: prophet_sm
date: 2025-06-05 11:40:13
tags: python
categories:
    - Python
---

# Prophet 요약

Prophet은 Facebook(현재 Meta)이 개발한 시계열 예측 라이브러리로, 불규칙한 시계열 데이터에서도 정확하고 해석 가능한 예측을 수행할 수 있도록 설계

특히 비즈니스 지표(예: 일별 사용자 수, 매출, 방문자 수 등) 예측에 강점

## 핵심 개념

Prophet 모델은 다음과 같은 구성요소를 기반으로 동작
```python
y(t) = g(t) + s(t) + h(t) + εt
```

- g(t): 트렌드 (비선형 또는 piecewise 선형)

- s(t): 주기적(계절성) 요소 (연간, 주간 등)

- h(t): 휴일 효과

- εt: 오류항 (노이즈)

## 주요 특징

| 항목       | 설명                                    |
| -------- | ------------------------------------- |
| 자동 추세 감지 | 트렌드 변화 시점을 자동으로 찾아 piecewise 선형 추세 적용 |
| 계절성 반영   | 주기성과 휴일 등의 외생 요인을 쉽게 반영 가능            |
| 결측치 처리   | 결측 데이터에 강건                            |
| 이상치 허용   | 이상치가 있어도 예측이 무너지지 않음                  |
| 인터랙티브 튜닝 | 계절성 주기, 휴일 영향, 추세 변화 민감도 등을 손쉽게 설정    |


## 사용 예제 (Python)

```python
from prophet import Prophet
import pandas as pd

# 데이터 형식: 'ds'는 날짜, 'y'는 예측 대상 변수
df = pd.read_csv('example.csv')  # 예: columns=['ds', 'y']

# 모델 생성 및 학습
model = Prophet()
model.fit(df)

# 미래 데이터프레임 생성 (예: 30일 예측)
future = model.make_future_dataframe(periods=30)
forecast = model.predict(future)

# 시각화
model.plot(forecast)
model.plot_components(forecast)
```

## 실전 적용 예시 (게임 데이터 기준)

| 사용 사례      | 설명                        |
| ---------- | ------------------------- |
| DAU/MAU 예측 | 일별/월별 유저 수 추세 예측          |
| 매출 예측      | 이벤트나 업데이트 주기에 따른 수익 변화 예측 |
| 리텐션 변화 감지  | 사용자 이탈 또는 회귀 경향의 탐지       |
| 이벤트 효과 평가  | 특정 기간 이벤트 전후의 지표 변화 비교    |


## 한계 및 주의사항

- 트렌드 전환점이 너무 잦으면 오버피팅 가능성 있음.

- 장기 예측은 불확실성 커짐: 1년 이상 예측 시 불확실성이 커짐.

- 외생 변수 반영 어려움: 경쟁사 출시 등 외부 충격은 직접 반영 어려움 (변수로 추가해야 함).