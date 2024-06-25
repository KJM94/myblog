---
title: Feature Engineering_RE
date: 2024-06-25 11:49:32
tags: python
categories:
    - Python
---
# Feature Engineering

기존의 포스팅은 당시 Kaggle 공모전에 앞서 내 할 것만 포스팅 한 느낌이라 다시 정리

- 피쳐 엔지니어링은 머신러닝 모델의 성능을 향상시키기 위해 데이터를 준비하고 변환하는 과정

1. 데이터 수집 및 이해

- 데이터의 출처와 의미를 이해합니다.
- 데이터의 형식, 특성, 결측치 등을 파악합니다.

2. 데이터 전처리:

- 결측값 처리: 결측값을 제거하거나 적절한 값으로 대체합니다.
- 이상치 제거: 이상값을 탐지하고 처리합니다.
- 스케일링: 피쳐의 범위를 조정하여 모델이 더 잘 학습하도록 합니다 (예: Min-Max Scaling, Standard Scaling).

3. 피쳐 선택:

- 피쳐 중요도 평가: 모델 성능에 큰 영향을 미치는 피쳐를 선택합니다.
- 차원 축소: PCA와 같은 방법을 사용하여 피쳐의 수를 줄입니다.

4. 피쳐 생성:

- 파생 피쳐 생성: 기존 피쳐를 조합하거나 변환하여 새로운 피쳐를 만듭니다 (예: 날짜 피쳐로부터 연, 월, 일 피쳐 생성).
- 상호작용 피쳐: 두 개 이상의 피쳐를 결합하여 새로운 피쳐를 만듭니다.

5. 변환 및 인코딩:

- 카테고리형 피쳐 인코딩: 원-핫 인코딩, 라벨 인코딩 등을 사용하여 범주형 데이터를 수치형 데이터로 변환합니다.
- 로그 변환, 제곱근 변환: 특정 피쳐의 분포를 정규화합니다.

6. 피쳐 선택 및 평가:

- 피쳐 선택 알고리즘: L1 정규화, 랜덤 포레스트 중요도, RFE (Recursive Feature Elimination) 등을 사용하여 중요한 피쳐를 선택합니다.
- 교차 검증: 다양한 피쳐 조합을 사용하여 모델을 평가합니다.

다음 예제는 피쳐 엔지니어링의 여러 단계를 포함하여 데이터 전처리와 모델 학습을 수행하는 과정 

StandardScaler를 사용하여 수치형 피쳐를 스케일링하고, 
<br>OneHotEncoder를 사용하여 범주형 피쳐를 인코딩하며, 
<br>ColumnTransformer를 사용하여 이를 결합합니다. 
<br>최종적으로 LogisticRegression 모델을 사용하여 분류 작업

```python
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler, OneHotEncoder
from sklearn.compose import ColumnTransformer
from sklearn.pipeline import Pipeline
from sklearn.linear_model import LogisticRegression

# 데이터 로드
data = pd.read_csv('data.csv')

# 피쳐와 타겟 분리
X = data.drop('target', axis=1)
y = data['target']

# 학습 및 테스트 데이터 분리
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# 수치형 및 범주형 피쳐 식별
numeric_features = ['age', 'income']
categorical_features = ['gender', 'occupation']

# 파이프라인 설정
numeric_transformer = Pipeline(steps=[
    ('scaler', StandardScaler())
])

categorical_transformer = Pipeline(steps=[
    ('onehot', OneHotEncoder(handle_unknown='ignore'))
])

preprocessor = ColumnTransformer(
    transformers=[
        ('num', numeric_transformer, numeric_features),
        ('cat', categorical_transformer, categorical_features)
    ])

# 모델 파이프라인 설정
model = Pipeline(steps=[
    ('preprocessor', preprocessor),
    ('classifier', LogisticRegression())
])

# 모델 학습
model.fit(X_train, y_train)

# 모델 평가
score = model.score(X_test, y_test)
print(f'Accuracy: {score}')
```