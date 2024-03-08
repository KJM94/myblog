---
title: Mysql_Pivot
date: 2024-03-08 12:48:16
tags: SQL
categories:
    - Sql
---
# PIVOT

- 데이터 회전
- 테이블의 데이터를 행에서 열로 변환

- MySQL에는 피벗 테이블을 생성하는 함수가 없음
- 따라서 사용자가 직접 쿼리문 작성 할 필요가 있음

다음은 필자의 간단한 예시


```
use world;

-- 예시 데이터
CREATE TABLE sales (
    product VARCHAR(50),
    category VARCHAR(50),
    amount DECIMAL(10, 2)
);

INSERT INTO sales (product, category, amount) VALUES
    ('ProductA', 'Category1', 100.00),
    ('ProductB', 'Category1', 150.00),
    ('ProductA', 'Category2', 200.00),
    ('ProductB', 'Category2', 250.00);

-- 피벗
SELECT
    product,
    SUM(CASE WHEN category = 'Category1' THEN amount END) AS Category1,
    SUM(CASE WHEN category = 'Category2' THEN amount END) AS Category2
FROM
    sales
GROUP BY
    product;

```

![피벗 전](/image/PI.PNG)

![피벗 후](/image/PIA.PNG)