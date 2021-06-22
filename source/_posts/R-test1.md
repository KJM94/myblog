---
title: R test1
date: 2021-03-23 16:21:09
tags: R
categories:
    - R
---

## 패키지 설치 방법
- ggplot2와 dplyr 패키지 설치 방법 기재

```
install.package("ggplot2")
install.package("dplyr")
```
패키지 설치
```r
library(ggplot2)
library(dplyr)
```
라이브러리 연결

## 액셀 데이터 받아오기

```r
counties <- readxl::read_xlsx("파일위치/파일이름.xslx", sheet = 1)
glimpse(counties)
```


getwd()로 위치확인 후 진행할 것에 주의
glimpse는 현재 데이터를 보여줌

## private_work, unemployment를 활용하여 산점도를 작성
- region을 기준으로 그룹화

```r
ggplot(data = counties, # 데이터
       aes(x = private_work, #x축
           y = unemployment, #y축 
           colour = region)) # 색상 = region 4개
geom_point()
```
![000002](https://user-images.githubusercontent.com/59479116/121620214-e1ff8a00-caa4-11eb-9698-91fc34fec3df.png)

## dplyr 함수를 활용하여, 아래 데이터 요약
- counties 데이터를 활용합니다.
- 변수 추출은 region, state, men, women
- 각 region, state 별 men, wemen 전체 인구수를 구함.
- 최종 데이터셋 저장 이름은 final_df로 명명

```r
counties %>%
  select(region, state, county, men, women) %>% # select
  group_by(region, state) %>% # group_by
  summarize(total_men = sum(men), #summarize
            total_women = sum(women)) %>% # head()여기까지 끊는 함수
  filter(region == "North Central") %>%
  arrange(desc(total_men)) -> final_df

glimpse(final_df)
```


## final_df를 기준으로 막대 그래프를 그린다.
- x축 : state
- 1개의 region만 선택

```r
ggplot(final_df, aes(x = state, y = total_men)) +
geom_col()
```

![000003](https://user-images.githubusercontent.com/59479116/121620233-efb50f80-caa4-11eb-9dd1-fb8767099e8b.png)

