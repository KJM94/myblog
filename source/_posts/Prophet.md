---
title: Prophet
date: 2024-01-31 10:21:01
tags: python
categories:
    - Python
---
# Prophet 모델

2017년 메타에서 공개한 시계열 예측 방법

- 시계열 방법에 대한 교육을 받지 않은 비전문가도 사용할 수 있어야 한다.
- 잠재적 특징들을 시계열 모델에 반영할 수 있어야 한다.
- 예측을 평가하고 다양하게 비교되도록 자동화되어야 한다.

![Figure 1: Schematic view of the analyst-in-the-loop approach to forecasting at scale, which best makes use of human and automated tasks.](/image/download(1).png)

분석과정을 자동으로 진행되며 결과만 확인하면 됨

![Figure 2: The number of events created on Facebook.](/image/download(2).png)

Figure2에서 시계열 데이터가 가질 수 있는 계절적 요소를 확인할 수 있는데, 주간/연간의 주기성이나 새해 전으로 하강하는 모습이 있다거나 최근 6개월 동안은 과거와는 달리 과도하게 변화하는 양상을 보이고 있음. 시계열 데이터에서 많이 보이며 완전히 자동화된 방법으로 이러한 데이터를 예측하는 것을 한계가 있음.

![Figure 3: Forecasts on the time series from Fig. 2 using a collection of automated forecasting procedures.](/image/download(3).png)

자동화된 방법들의 한계
각 모델의 작동을 이해하는데 일반적인 분석가들은 어려움을 겪을 것

![](/image/download(4).png)

트렌드(growth), 계절성(seasonality), 휴일(holidays)

g(t)는 비주기적 변화를 반영하는 추세 함수, s(t)는 주기적인 변화(ex, 주간/연간)를, h(t)는 휴일

- 유연성 : 계절적 요소와 트렌드에 대한 가정을 쉽게 반영할 수 있다
- ARIMA 모델과 달리 모델을 측정 간격을 regularly spaced 할 필요가 없고, 결측이 있어도 상관없다
- 빠른 fitting 속도 -> 여러 가지 시도해볼 수 있다
- 직관적인 파라미터 조정을 통한 모델 확장이 쉽다

## Nonlinear, Saturating Growth

예측하고자 하는 값의 한계가 존재하는 경우

![](/image/download(5).png)

![로지스틱 학수](/image/download(6).png)

C는 한계, K는 성장률

반영할 수 없는 부분

- C가 상수가 아닌 경우
    - 페이스북 가입자 수를 예측한다고 할 때, 우리의 상한은 인터넷에 연결이 가능한 인구의 수이지만,
인터넷 연결이 가능한 인구는 시간이 지남에 따라 증가하기 때문에 상수로 볼 수 없음
- 성장률이 상수가 아닌 경우
    - 신제품 출시로 기존의 성장률과 다른 양상을 보일 수 있음

## Linear Trend with Changpoints

![](/image/download(7).png)

![](/image/download(8).png)

어떠한 한계가 존재하지 않는 예측 문제에 있어서 piecewise linear 모델은 간결하고 유용

![The Trend model](/image/download(9).png)

changpoints는 자동 탐지되며, 실제 적용 시에는 그 정도(flexibility)를 설정해줄 수 있다

주기적 패턴의 근사치 찾기
![The standard Fourier series](/image/download(10).png)

- P : 정규 주기
- N이 커질수록 패턴 감지력이 정교해짐
![Fourier Order for Seasonalities with N=10 and N=20 respectively](/image/download(11).png)

휴일과 이벤트는 주기적 패턴이 없어 모델링 난이도가 높음
그러나 시계열에 미치는 영향은 매년 비슷하기 때문에 반영되어야 함
휴일을 Di로 두었을 때 h(t) 정의
![](/image/download(12).png)
![](/image/download(13).png)

![](/image/download(14).png)

한국의 휴일은 holiday 패키지에 정의되어 있음

## Model Fitting

아래의 Figure 4와 5는 prophet을 이용해 각각 Figure 3과 동일한 범위를 예측, 전제 데이터로 모든 시간에 대해 예측한 결과

![Figure 4: Prophet forecasts corresponding to those of Fig. 3. As before, forecasts are grouped by day-of-week to visualize weekly seasonality.](/image/download(15).png)
![Figure 5: Prophet forecast using all available data, including the interpolation of the historical data. Solid lines are in-sample fit, dashed lines are out-of-sample forecast.](/image/download(16).png)

분해가 가능한 모델의 중요한 점은 예측의 각 구성요소를 확인할 수 있음

아래처럼 트렌드와 계절성(주 단위, 연단위) component가 어떤 영향을 미치고 있는지 확인할 수 있는 그래프도 확인할 수 있음

![Figure 6: Components of the Prophet forecast in Fig. 5.](/image/download(17).png)

출처 : https://slowsteadystat.tistory.com/7
<br>
참고자료 : https://facebook.github.io/prophet/docs/quick_start.html
<br>
직접 작성해본 코드 : https://github.com/KJM94/career/blob/main/py/Prophet.ipynb