---
title: twitter API prac
date: 2021-03-26 16:21:30
tags: R
---

# 트위터 API 인증
- https://apps.twitter.com
- 회원가입 한 뒤 create an app 클릭 후 진행

# rtweet 패키지

```r
# install.packages("rtweet")
library(rtweet)
library(dplyr)
library(ggplot2)
```

# search Tweets 함수

```r
rstats <- search_tweets("#테슬라", n = 1000, include_rts = FALSE) %>% 
  select(name, location, description)
```

```r
glimpse(rtats)
```

api 사용자 토큰이 없어서 결과를 올리지 못함.