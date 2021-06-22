---
title: R aes
date: 2021-03-23 12:19:54
tags: R
categories:
    - R
---

getwd() 위치확인
rm(list = ls()) 모두 지우기

```r
library(ggplot2)

ggplot(iris, aes(x = Sepal.Length, y = Sepal.Width,
                  colours = Species)) + 
  geom_point(size = 5) + 
  theme_bw()

ggplot(iris, aes(x = Sepal.Length,
                 y = Sepal.Width,
                 size = Petal.Length,
                 colours = Species)) + 
  geom_point() + 
  theme_bw()

ggplot(iris, aes(x = Sepal.Length,
                 y = Sepal.Width,
                 colours = Species)) + 
  geom_point(size = 2) +
  theme_bw()
```

aes(size  = Petal.Lengh, colour = Species)
전체 옵션 선택 : aes() 밖에서  = 그래프 종류
각각의 데이터별로 옵션을 선택
aes(안에서 옵션 설정, 변수명)
customize --> scale

