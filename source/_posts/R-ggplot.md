---
title: R-ggplot No.1
date: 2021-03-24 15:40:25
tags: R
categories:
    - R
---

### 눈금 표시 방법
```r
ggplot(PlantGrowth, 
       aes(x = group, y = weight)) +
  geom_boxplot() +
  scale_y_continuous(limits = c(0, 10), # 축 범위
                     breaks = c(1, 3, 5, 7, 9), # 축의 숫자 지정
                     labels = c("1st", "three", "five", "seven", "nine"))   
```
![834e0eea-c2a5-457f-8db3-5be48606f893](https://user-images.githubusercontent.com/59479116/121620298-0eb3a180-caa5-11eb-8016-c53d09960339.png)

### 범주형 축 항목 순서 변경하기
```r
ggplot(PlantGrowth, aes(x = group, weight)) +
  geom_boxplot() +
  theme_bw() +
  scale_x_discrete(limits = c("trt1", "ctrl"))
```

![70048f9e-2d1b-4ae3-9cec-81d31bf5bdcd](https://user-images.githubusercontent.com/59479116/121620331-1c692700-caa5-11eb-8d53-45f8d27e2ff5.png)

