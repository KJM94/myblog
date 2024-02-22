---
title: Random Forest
date: 2024-02-22 10:43:47
tags: python
categories:
    - Python
---
# 랜덤 포레스트

의사결정 트리를 구성하여 개별 트리의 클래스 분류 또는 회귀 분석 앙상블 학습 알고리즘

### 의사결정 트리
- 각 트리는 특성의 하위 집합과 데이터의 무작위 하위 집합을 사용하여 구성

### 부트스트랩
- 의사결정 트리를 독립적으로 학습시키는 배깅 기술 사용
- 각 트리에 대해 원본 데이터 세트의 무작위 샘플이 사용

### 무작위 기능 하위 집합
- 각 트리는 임의의 기능 하위 집합에 대해 학습

### 분류 또는 회귀
- 분류 : 트리 중 다수결로 최종 예측
- 회귀 : 개별 트리의 예측 평균

## 특징

1. 높은 정확도
2. 과적합 감소
3. 이상값과 노이즈에 강력
4. 특성 중요도 정보 제공

```python
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score
from sklearn.datasets import load_iris

# 아이리스 데이터셋 불러오기
iris = load_iris()
X = iris.data
y = iris.target

# 테스트셋과 트레이닝셋 나누기
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# 랜덤 포레스트 분류
rf_classifier = RandomForestClassifier(n_estimators=100, random_state=42)

# 학습
rf_classifier.fit(X_train, y_train)

# 예측
predictions = rf_classifier.predict(X_test)

# 정확도 측정
accuracy = accuracy_score(y_test, predictions)
print(f'Accuracy: {accuracy}')
```
![](/image/ac.PNG)

정확도가 1이 나오는 것은 이미 학습한 데이터를 기반으로 예측하기 때문이며<br>
실제 사용에 있어서는 학습하지 않은 데이터를 예측해야 하기 때문에<br>
1이 나오는 것은 매우 어려움
