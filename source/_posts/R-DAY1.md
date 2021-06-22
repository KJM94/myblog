---
title: R-DAY1
date: 2021-03-22 14:28:16
tags: R
categories:
    - R
---

```R
# Factor 범주형 변수
# 컴퓨터 <- 숫자로 인식
# 문자 숫자 논리형 순으로 저장
# ordered = TRUE
# levels = c("정렬")
```

```R
# 1. 사람과 관련된 데이터
# - 고객 데이터 (CRM)
# 2. 기계와 관련된 데이터
# - 제조공정과 관련된 데이터
# install.packages("패키지") 패키지 설치

# 1단계 : 패키지 불러오기
install.packages("ggplot2")
library(ggplot2)

# 2단계 : 데이터 불러오기
data("iris")

# 3단계 : 데이터 확인
str(iris)

# 4단계 : 데이터 가공 하기 (시각화)

# 5단계 : 시각화
ggplot(data = iris, mapping = aes(x = Petal.Length, y = Petal.Width)) + geom_point() +
  geom_smooth(method="loess", se=F) +
  #xlim(c(0, 0.1)) + 
  #ylim(c(0, 500000)) + 
  labs(subtitle="Area Vs Population",
       y="Population",
       x="Area",
       title="Scatterplot",
       caption = "Source: midwest")
```
