---
title: Feature Engineering
date: 2021-04-08 21:48:11
tags: python
categories:
    - Python
---

1) 데이터 수집
2) 시각화
    -> 변수 간의 조합
3) 기초통계 : Feature Engineering

Feature Engineering
- 이상치 처리, 중복값 제거, 문자 데이터 --> 수치 (인코딩)
- 정규화, 표준화, 도출 변수 및 불필요한 삭제
- PCA(차원 축소),EFA
- 결측치 확인
    : 1) 결측치 제거
    : 2) 결측치 채우기
        - 중간값, 빈도수 값 많은 것 넣기
        - Imputation
    : 3) 결측치 도메인 활용
- 왜도처리(Skewnewss)
- 도출 변수
- Label Encoding --> 종속변수에만 사용
- Ordinal Encoding --> 독립변수에만 사용
--> 문자를 숫자로 바꿈
--> Default, 가~하, A-Z 순으로 숫자 바뀜
--> 순서형

- One - Hot Encoding --> 독립변수에만 사용
--> 명목형

4) 훈련 데이터 / 테스트 데이터
5) 모형 학습 (다양한 모형 공부 필요)
6) 예측 결과
7) 예측 결과 평가
8) 최종 모델 선정 및 최종 예측 결과
9) 캐글 대회 제출
--> 주 목적, 예측 정확성 향상상) 최종 모델 선정 및 최종 예측 결과
9) 캐글 대회 제출
--> 주 목적, 예측 정확성 향상
