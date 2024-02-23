---
title: ANOVA
date: 2024-02-23 14:51:39
tags: python
categories:
    - Python
---
# 분산 분석
- 세 개 이상의 독립 그룹의 평균 간에 통계적으로 유의미한 차이 여부 평가
- 그룹 간 변동성, 그룹 내 변동성

## 귀무가설(H0)
- 모든 그룹의 평균이 동일하다고 가정

## 대립가설(H1)
- 적어도 하나의 그룹 평균이 다른 그룹 평균과 다름

## F-통계
- 분산의 비율
- 값이 크다면 그룹 간 병동성이 그룹 내 변동성보다 크다는 것을 나타냄

## 자유도
- 그룹 간 자유도는 그룹 수에서 1을 뺀 것과 같음
- 그룹 내 자유도는 총 관측치 수에서 그룹 수를 뺀 값과 같음

## p-value
- 통계적 유의성 결정
- p-값이 유의수준(일반적으로 0.05)보다 낮으면 귀무 가설 기각

```python
from scipy.stats import f_oneway

# 예시 그룹 데이터
group1 = [1, 2, 3, 4, 5]
group2 = [2, 3, 4, 5, 6]
group3 = [3, 4, 5, 6, 7]

# 단방향 분산분석
f_statistic, p_value = f_oneway(group1, group2, group3)

# 결과
print(f'F-Statistic: {f_statistic}')
print(f'p-value: {p_value}')

# 가설 확인
alpha = 0.05
if p_value < alpha:
    print('귀무가설을 기각합니다. 그룹 평균 간에는 상당한 차이가 있습니다.')
else:
    print('귀무가설을 기각하지 않습니다. 그룹 평균 간에는 큰 차이가 없습니다.')
```

![](/image/an.PNG)
