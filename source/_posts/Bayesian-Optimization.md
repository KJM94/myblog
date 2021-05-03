---
title: Bayesian Optimization
date: 2021-04-29 11:34:54
tags: python
---

# 베이지안 최적화

- 불필요한 하이퍼 파라미터 반복 탐색을 줄여 빠르게 최적의 하이퍼 파라미터를 찾을 수 있는 방법
- 하이퍼 파라미터를 선택하는 문제에 대하여 정형화된 방법을 찾지 못해, 경험에 의해서 선택해야 함

사용하는 이유

- 그리드 서치 or 랜덤 서치는 도출된 하이퍼 파라미터 값을 일일이 모델에 적용한 뒤 성능 비교를 해야 하는 부분이 존재하지만 베이지안 최적화는 사용자가 설정한 파라미터 구간내에서 최적의 값을 도출함

## 베이지안 최적화에 사용되는 핵심 모듈

1. Surrogate model

- 확보된 데이터와 평가지표의 숨겨진 관계를 모델링

2. Acquisition function

- Surrogate model을 활용해 다음 탐색할 지점을 결정

베이지안 최적화는 사전에 정의된 하이퍼파라미터 집합으로부터 일반화 성능을 도출하는 surrogate model과 acquisition function을 사용하여 최적의 조합 결정


베이지안 최적화 과정

```python
lgb_params = {
    'num_leaves': (2, 50),
    'colsample_bytree':(0.1, 1), 
    'subsample': (0.1, 1),
    'max_depth': (1, 50),
    'reg_alpha': (0, 0.5),
    'reg_lambda': (0, 0.5), 
    'min_split_gain': (0.001, 0.1),
    'min_child_weight':(0, 50),
    'subsample_freq': (2, 50),
    'max_bin': (5,200),
}
```

- 일정량의 hyperparameter 세팅을 X 변수로 하고

```python
def lgb_roc_eval(num_leaves, colsample_bytree, subsample, max_depth, reg_alpha, reg_lambda, min_split_gain, min_child_weight, subsample_freq, max_bin):
    
    params = {
        'learning_rate':0.01,
        'num_leaves': int(round(num_leaves)),   #  호출 시 실수형 값이 들어오므로 정수형 하이퍼 파라미터는 정수형으로 변경 
        'colsample_bytree': colsample_bytree, 
        'subsample': subsample,
        'max_depth': int(round(max_depth)),
        'reg_alpha': reg_alpha,
        'reg_lambda': reg_lambda, 
        'min_split_gain': min_split_gain,
        'min_child_weight': min_child_weight,
        'subsample_freq': int(round(subsample_freq)),
        'max_bin': int(round(max_bin)),
    }
    lgb_model = LGBMClassifier(**params)
    lgb_model.fit(bayes_x, bayes_y, eval_set=[(bayes_x_test, bayes_y_test)], early_stopping_rounds=100, eval_metric="auc", verbose=False)
    valid_proba = lgb_model.predict_proba(bayes_x_test, num_iteration=10)[:,1]
    roc_preds = roc_auc_score(bayes_y_test, valid_proba)
    
    return roc_preds
```

- 모델의 성능을 Y변수로 하여 데이터 셋을 만든다

```python
BO_lgb = BayesianOptimization(lgb_roc_eval, lgb_params, random_state=2121)
```

```python
BO_lgb.maximize(init_points=5, n_iter=10)


BO_lgb.max 
```

```python
|   iter    |  target   | colsam... |  max_bin  | max_depth | min_ch... | min_sp... | num_le... | reg_alpha | reg_la... | subsample | subsam... |
-------------------------------------------------------------------------------------------------------------------------------------------------
|  1        |  0.8342   |  0.329    |  196.3    |  41.25    |  45.29    |  0.06231  |  28.93    |  0.1433   |  0.4287   |  0.3861   |  46.96    |
|  2        |  0.838    |  0.6284   |  15.83    |  39.32    |  44.95    |  0.04359  |  24.72    |  0.4127   |  0.000694 |  0.7192   |  13.98    |
|  3        |  0.8338   |  0.3291   |  72.7     |  38.96    |  25.69    |  0.06552  |  20.02    |  0.03046  |  0.3621   |  0.54     |  12.71    |
|  4        |  0.8277   |  0.2622   |  34.52    |  13.99    |  3.131    |  0.0492   |  11.02    |  0.1738   |  0.4319   |  0.308    |  17.25    |
|  5        |  0.8271   |  0.4439   |  138.9    |  9.848    |  48.94    |  0.02053  |  14.95    |  0.4408   |  0.213    |  0.8062   |  14.36    |
|  6        |  0.8363   |  0.6169   |  74.36    |  36.76    |  28.7     |  0.07251  |  19.8     |  0.2751   |  0.05854  |  0.3577   |  13.47    |
|  7        |  0.761    |  0.1915   |  13.88    |  33.33    |  44.99    |  0.06156  |  29.77    |  0.3761   |  0.4008   |  0.801    |  8.465    |
|  8        |  0.8006   |  0.1517   |  72.77    |  32.76    |  25.55    |  0.05765  |  22.96    |  0.03691  |  0.3594   |  0.9731   |  14.69    |
|  9        |  0.8348   |  0.9296   |  74.38    |  40.81    |  29.16    |  0.024    |  16.55    |  0.3314   |  0.1303   |  0.75     |  15.69    |
|  10       |  0.8271   |  0.4366   |  17.04    |  39.74    |  46.38    |  0.09246  |  23.49    |  0.1639   |  0.09529  |  0.518    |  19.89    |
|  11       |  0.8382   |  0.5201   |  16.37    |  39.06    |  48.15    |  0.05973  |  19.88    |  0.3476   |  0.06915  |  0.6018   |  13.36    |
|  12       |  0.7623   |  0.1818   |  75.99    |  41.25    |  32.32    |  0.04383  |  22.84    |  0.4678   |  0.4575   |  0.4314   |  12.78    |
|  13       |  0.8294   |  0.2367   |  15.62    |  40.93    |  41.88    |  0.09569  |  18.08    |  0.4144   |  0.2859   |  0.6626   |  11.77    |
|  14       |  0.7633   |  0.1896   |  197.7    |  41.72    |  41.81    |  0.0144   |  29.92    |  0.1247   |  0.1963   |  0.2601   |  43.21    |
|  15       |  0.8383   |  0.838    |  75.9     |  37.78    |  28.52    |  0.04416  |  15.26    |  0.3148   |  0.3394   |  0.736    |  14.43    |
=================================================================================================================================================
{'target': 0.8382516227577165,
 'params': {'colsample_bytree': 0.8380432818402,
  'max_bin': 75.89932895205534,
  'max_depth': 37.77537008850686,
  'min_child_weight': 28.524551188271897,
  'min_split_gain': 0.04415839847288688,
  'num_leaves': 15.259817309189941,
  'reg_alpha': 0.31476726664722326,
  'reg_lambda': 0.3394257380973246,
  'subsample': 0.7360076401454746,
  'subsample_freq': 14.425454096134617}}
```

- 해당 데이터셋에서 X와 Y의 관계를 가지고 사전 지식을 만든 후 새로운 데이터(X변수, hyperparameter 조합)이 들어 왔을 때 일반화 성능이 우수하도록 하는 최적의 x를 찾아서 추가해주는 형식으로 진행

위의 프로세스에서 surrogate model과 acquistion function 등장하는데

- surrogate model은 X변수를 입력받아 일반화 성능과 불확실성에 관련된 분포를 내뱉는 함수
- Acquisition function은 가장 최적의 x값을 찾는 함수

## 장점

- 탐색 시간적인 측면에서 효율을 가질 수 있음

## 단점

- hyperparameter가 증가하면 차원이 급격하게 늘어나 성능을 예측하는 것이 점점 어려움


참고자료 : https://www.kaggle.com/elon4773/titanic-visualization-bayesian-optimization