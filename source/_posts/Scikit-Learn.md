---
title: Scikit-Learn
date: 2024-02-16 10:02:15
tags: python
categories:
    - Python
---
# Scikit-Learn

- Python 오픈 소스 머신러닝 라이브러리
- Numpy, SciPy, Matplotlib 등 Python 라이브러리 기반

## 기능
- 분류 알고리즘(ex. SVM, 의사결정 트리, 랜덤 포레스트)
- 회귀 알고리즘(ex. 선형 회귀, 능선 회귀)
- 클러스터링(ex. K-평균, 계층적 클러스터링)
- 차원 축소(ex. PCA - 주성분 분석)
- 데이터 전처리, 모델 선택 및 평가

```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error

# 임의의 데이터 생성
np.random.seed(42)
X = 2 * np.random.rand(100, 1)
y = 4 + 3 * X + np.random.randn(100, 1)

# 훈련, 테스트 데이터셋 분할
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# 선형 회귀 모델
model = LinearRegression()

# 훈련
model.fit(X_train, y_train)

# 예측
y_pred = model.predict(X_test)

# 평가
mse = mean_squared_error(y_test, y_pred)
print(f'Mean Squared Error: {mse}')

# 시각화
plt.scatter(X_train, y_train, label='Training Data')
plt.scatter(X_test, y_test, color='red', label='Test Data')
plt.plot(X_test, y_pred, color='blue', linewidth=3, label='Regression Line')
plt.xlabel('X')
plt.ylabel('y')
plt.legend()
plt.show()
```

![](/image/s.PNG)