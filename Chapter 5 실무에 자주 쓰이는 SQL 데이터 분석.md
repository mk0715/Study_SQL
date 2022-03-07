# 1. Excel 데이터 분석을 SQL로

## 1-1 ERD를 활용한 데이터 마트 구성
- ERD(Entity-Relationship Diagram)은 ERM 프로세스의 최종 산출물.
- ERM : 데이터를 구조화하기 위한 개체-관계 모델링 기법.

![image](https://user-images.githubusercontent.com/75361137/155000895-b686d7f8-d46c-4b6e-a306-ad9f34822971.png)

- 데이터 마트 구성
1. [Car_order] 테이블을 왼쪽으로 하여 모든 테이블 LEFT JOIN
2. [Car_orderdetail]의 주문수량(quantity) 및 [Car_product]의 가격(price)을 곱하여 주문금액(sales_amt) 열을 생성
3. [Car_MART] 테이블 생성

```sql
USE EDU

-- EDU를 활용한 데이터 마트 구성
SELECT A.*,
       B.prod_cd,
       B.quantity,
       D.price,
       B.quantity * D.price AS sales_amt,
       C.store_addr,
       D.brand,
       D.model,
       E.gender,
       E.age,
       E.addr,
       E.join_date
INTO [Car_MART]
FROM [Car_order] A
LEFT JOIN [Car_orderdetail] B ON A.order_no = B.order_no
LEFT JOIN [Car_store] C ON A.store_cd = C.store_cd
LEFT JOIN [Car_product] D ON B.prod_cd = D.prod_cd
LEFT JOIN [Car_member] E ON A.mem_no = E.mem_no

-- [Car_MART] 테이블의 모든 컬럼 조회
SELECT *
FROM [Car_MART]
```
- LEFT JOIN 되는 테이블의 열 이름을 작성한 이유는 중복되는 열 이름이 있으면 테이블이 생성되지 않기 때문.
- sales_amt(주문금액)은 주문수량(B.quantity) * 가격(D.price)


## 1-2 구매 고객 프로파일 분석
- 구매 고객 프로파일 분석은 고객들의 성별 / 연령 / 지역 등을 활용해 구매자의 특징 및 특성을 파악하고자 할 때 사용.

### 1-2-1 [Car_MART] 테이블에 연령대 열을 추가한 세션 임시 테이블 생성
- 조건 : 연령구간은 10세 / 열 이름 : [ageband]
- 임시 테이블명 : #PROFILE_BASE

```sql
USE EDU

-- 데이터 마트 활용 : [Car_MART]

-- 구매 고객 프로파일 분석
-- 연령대(ageband) 열 추가한 세션 임시 테이블(#PROFILE_BASE) 생성
SELECT *,
       CASE WHEN age < 20 THEN '20대 미만'
            WHEN age BETWEEN 20 AND 29 THEN '20대'
            WHEN age BETWEEN 30 AND 39 THEN '30대'
            WHEN age BETWEEN 40 AND 49 THEN '40대'
            WHEN age BETWEEN 50 AND 59 THEN '50대'
            ELSE '60대 이상' END AS ageband
INTO #PROFILE_BASE
FROM [Car_MART]

-- 세션 임시 테이블(#PROFILE_BASE) 조회
SELECT *
FROM #PROFILE_BASE
```

- 임시 테이블 : 영구적으로 테이블을 생성할 필요가 없을 때 임시로 사용하는 테이블.

| 테이블 | 사용 위치 | 생성 방법 | 삭제 방법 |
| ---------------- | ---------------- | ---------------- | ---------------- |
| 일반 테이블 | 영구적으로 사용 | CREATE TABLE [테이블명] | DROP TABLE [테이블명] |
| 세션 임시 테이블 | 해당 쿼리창 | INTO #[테이블명] | DROP TABLE #[테이블명] 또는 해당 쿼리 창 종료 |
| 전역 임시 테이블 | 모든 쿼리창 | INTO ##[테이블명] | DROP TABLE ##[테이블명] 또는 모든 쿼리 창 종료 |


### 1-2-2 #PROFILE_BASE 테이블을 활용한 성별 및 연령대별 구매자 분포

```sql
-- 성별 구매자 분포
SELECT gender, COUNT(DISTINCT mem_no) AS tot_mem
FROM #PROFILE_BASE
GROUP BY gender

-- 연령대별 구매자 분포
SELECT ageband, COUNT(DISTINCT mem_no) AS tot_mem
FROM #PROFILE_BASE
GROUP BY ageband

-- 성별 및 연령대별 구매자 분포
SELECT gender, ageband, COUNT(DISTINCT mem_no) AS tot_mem
FROM #PROFILE_BASE
GROUP BY gender, ageband
ORDER BY 1
```
- COUNT(DISTINCT mem_no)는 중복을 제거한 구매자 수.

- 엑셀로 분석한 것과 결과가 같음.(그래프 제외, 결과만)

![image](https://user-images.githubusercontent.com/75361137/155064108-65fb3d5b-8d29-4483-a756-765a572b4a8f.png)

![image](https://user-images.githubusercontent.com/75361137/155064120-c67b4b9e-35c2-47e3-91ae-aa75ad02792f.png)


### 1-2-3 #PROFILE_BASE 테이블을 활용한 성별 및 연령대별 구매자 분포 (+연도별)

```sql
-- 성별 및 연령대별 구매자 분포 (+연도별)
SELECT gender, ageband, 
       COUNT(DISTINCT CASE WHEN YEAR(order_date) = 2020 THEN mem_no END) AS tot_mem_2020,
       COUNT(DISTINCT CASE WHEN YEAR(order_date) = 2021 THEN mem_no END) AS tot_mem_2021
FROM #PROFILE_BASE
GROUP BY gender, ageband
ORDER BY 1
```
- CASE WHEN에서 ELSE로 값을 지정하지 않으면, 조건 외 값은 NULL로 반환.
- COUNT는 NULL을 제외하고 개수를 집계.

- 엑셀로 분석한 것과 결과가 같음.(그래프 제외, 결과만)

![image](https://user-images.githubusercontent.com/75361137/155064469-775df7ca-0697-4772-8e3a-e9286defd368.png)


## 1-3 RFM 고객세분화분석
- RFM 고객세분화 분석은 고객들의 구매지표(최근성 / 구매빈도 / 구매금액) 등을 활용해 고객을 세분화하고자 할 때 사용.
- RFM : R(Recency) 최근성 / F(Frequency) 구매빈도 / M(Monetary) 구매금액

### 1-3-1 [Car_MART] 테이블을 활용해 고객별 RFM 세션 임시 테이블 생성
- 조건 : 주문일자(2020~2021) 동안의 구매빈도 및 금액
- 임시 테이블명 : #RFM_BASE

```sql
USE EDU

-- 데이터 마트 활용 : [Car_MART]

-- RFM 고객세분화 분석
-- R(Recency) : 최근성 / F(Frequency) : 구매빈도 / M(Monetary) : 구매금액

-- 고객(mem_no)별 RFM 구하기
SELECT mem_no,
       SUM(sales_amt) AS tot_amt, -- M : 구매금액
       COUNT(order_no) AS tot_tr -- F : 구매빈도
INTO #RFM_BASE
FROM [Car_MART]
WHERE YEAR(order_date) BETWEEN 2020 AND 2021 -- R : 최근성
GROUP BY mem_no

-- 세션 임시 테이블 (#RFM_BASE)
SELECT *
FROM #RFM_BASE
```
- 세션 임시 테이블이 생성되고 고객별 RFM이 조회.

### 1-3-2 [Car_member] 왼쪽 테이블 기준으로 #RFM_BASE를 LEFT JOIN 후, 고객세분화 그룹 열을 추가한 세션 임시 테이블 생성
- 고객세분화 그룹 조건(구매금액 및 빈도 기준)
1. VVIP : 구매금액 10억 이상 & 구매빈도 3회 이상
2. VIP : 구매금액 5억 이상 & 구매빈도 2회 이상
3. GOLD : 구매금액 3억 이상
4. SILVER : 구매금액 1억 이상
5. BRONZE : 구매빈도 1회 이상
6. POTENTIAL : 구매이력 없음
- 임시 테이블명 : #RFM_BASE_SEG

```sql
-- [Car_member] 왼쪽 테이블 기준으로 #RFM_BASE LEFT JOIN 후, 가테이블(#) 생성
SELECT A.*, B.tot_amt, B.tot_tr,
       CASE WHEN B.tot_amt >= 1000000000 AND B.tot_tr >= 3 THEN '1_VVIP'
            WHEN B.tot_amt >=  500000000 AND B.tot_tr >= 2 THEN '2_VIP'
            WHEN B.tot_amt >=  300000000 THEN '3_GOLD'
            WHEN B.tot_amt >=  100000000 THEN '4_SILVER'
            WHEN B.tot_tr >= 1 THEN '5_BRONZE'
            ELSE '6_POTENTIAL' END AS segmentation
INTO #RFM_BASE_SEG
FROM [Car_member] A
LEFT JOIN #RFM_BASE B
ON A.mem_no = B.mem_no

-- 세션 임시 테이블(#RFM_BASE_SEG) 조회
SELECT *
FROM #RFM_BASE_SEG
```
- Car_member 및 #RFM_BASE 테이블이 1:1관계(mem_no)로 LEFT JOIN.
- CASE WHEN으로 구매금액 및 빈도 조건별로 고객세분화 그룹 분리.
- Car_member 왼쪽 테이블 기준으로 #RFM_BASE 테이블이 LEFT JOIN 되었기 때문에 가입만 하고 구매이력이 없으면 POTENTIAL.

### 1-3-3 #RFM_BASE_SEG 테이블을 활용한 고객세분화(segmentation)별 고객 수 및 매출 비중 파악

```sql
-- 고객세분화(segmentation)별 고객 수 및 매출 비중
SELECT segmentation,
       COUNT(mem_no) AS tot_mem,
       SUM(tot_amt) AS tot_amt
FROM #RFM_BASE_SEG
GROUP BY segmentation
ORDER BY 1
```
- 엑셀로 분석한 것과 결과가 같음.(그래프 및 퍼센트 제외, 결과만)

![image](https://user-images.githubusercontent.com/75361137/155067951-4e15a16e-7eaf-4cea-9769-a7db372341b1.png)

![image](https://user-images.githubusercontent.com/75361137/155067980-c136c1d7-347d-47d9-94b8-cd94b9c7a063.png)


## 1-4 구매전환율 및 구매주기 분석
- 구매전환율 및 구매주기 분석은 고객들의 구매패턴을 파악하고자 할 때 사용.

### 1-4-1 [Car_MART] 테이블을 활용해, 구매전환율 세션 임시 테이블 생성
- 임시 테이블 명 : #RETENTION_BASE

```sql
USE EDU

-- 데이터 마트 활용 : [Car_MART]

-- 구매전환율 분석
-- 2020 구매자 중, 2021 구매여부 열 추가한 가테이블(#) 생성
SELECT A.mem_no AS pur_mem_2020,
       B.mem_no AS pur_mem_2021,
       CASE WHEN B.mem_no IS NOT NULL THEN 'Y' ELSE 'N' END AS retention_yn
INTO #RETENTION_BASE
FROM (SELECT DISTINCT mem_no FROM [Car_MART] WHERE YEAR(order_date) = 2020) A
LEFT JOIN (SELECT DISTINCT mem_no FROM [Car_MART] WHERE YEAR(order_date) = 2021) B
ON A.mem_no = B.mem_no

-- 세션 임시 테이블(#RETENTION_BASE) 조회
SELECT *
FROM #RETENTION_BASE
```
- 구매여부 열이 추가된 세션 임시 테이블 생성
- A(2020년 구매 고객) 왼쪽 테이블 기준으로 B(2021년 구매 고객) 테이블이 LEFT JOIN되었기 때문에 B.mem_no가 NULL이 아니면 구매 전환된 회원.

### 1-4-2 #RETENTION_BASE 테이블을 활용한 구매전환율 파악

```sql
-- 구매전환율 구하기
SELECT COUNT(pur_mem_2020) AS tot_mem,
       COUNT(CASE WHEN retention_yn ='Y' THEN pur_mem_2020 END) AS retention_mem
FROM #RETENTION_BASE
```
- 엑셀로 분석한 것과 결과가 같음.(퍼센트 제외, 결과만)

![image](https://user-images.githubusercontent.com/75361137/155079055-b7fe3c85-da01-429a-96af-a9ac22b8ab25.png)

### 1-4-3 [Car_MART] 테이블을 활용하여 매장코드(store_cd)별로 구매주기에 필요한 세션 임시 테이블 생성
- 구매주기에 필요한 값 : 최근 구매일, 최초 구매일, (구매횟수-1)
- 임시 테이블명 : #CYCLE_BASE

```sql
-- 매장코드(store_cd)별 구매주기
-- 구매주기에 필요한 값(최근 구매일, 최초 구매일, 구매횟수-1)
-- 구매횟수 2회 이상 필터링
SELECT store_cd,
       MIN(order_date) AS min_order_date,
       MAX(order_date) AS max_order_date,
       COUNT(DISTINCT order_no) -1 AS tot_tr_1
INTO #CYCLE_BASE
FROM [Car_MART]
GROUP BY store_cd
HAVING COUNT(DISTINCT order_no) >= 2

-- 세션 임시 테이블(#CYCLE_BASE) 조회
SELECT *
FROM #CYCLE_BASE
```
- 매장코드(store_cd)별 구매주기에 필요한 값(최근 구매일, 최초 구매일, 구매횟수-1)이 세션 임시 테이블로 생성.
- 구매횟수 2회이상으로 필터한 이유는 구매횟수 1회면 분모가 0(1-1=0)이기 때문.

### 1-4-4 #CYCLE_BASE 테이블을 활용한 구매주기 파악

```sql
-- 매장코드(store_cd)별 구매주기 구하기
SELECT *,
       DATEDIFF(DAY, min_order_date, max_order_date) AS diff_day,
       DATEDIFF(DAY, min_order_date, max_order_date) * 1.00 / tot_tr_1 AS cycle
FROM #CYCLE_BASE
ORDER BY 6 DESC
```
- 매장코드별 구매주기 파악.
- DATEDIFF 함수를 통해 [min_order_date] 및 [max_order_date] 일(DAY) 차이 파악.
- 1.00은 소수점까지 나타내기 위해 저렇게 표기.

- 엑셀로 분석한 것과 결과가 같음.(그래프 제외, 결과만)

![image](https://user-images.githubusercontent.com/75361137/155080183-0f281db1-5509-4792-b128-e2cbd56d8d8d.png)


## 1-5 제품 및 성장률 분석
- 제품 및 성장률 분석은 상품들의 경쟁력을 파악하고자 할 때 사용.

### 1-5-1 [Car_MART] 테이블을 활용하여 브랜드(brand) 및 모델(model)별 2020, 2021년 구매금액 세션 임시 테이블 생성
- 임시 테이블명 : #PRODUCT_GROWTH_BASE

```sql
USE EDU

-- 데이터 마트 활용 : [Car_MART]

-- 제품 및 성장률 분석
-- 브랜드 및 모델별 2020, 2021년 구매금액 가테이블(#) 생성
SELECT brand, model,
       SUM(CASE WHEN YEAR(order_date) = 2020 THEN sales_amt END) AS tot_amt_2020,
       SUM(CASE WHEN YEAR(order_date) = 2021 THEN sales_amt END) AS tot_amt_2021
INTO #PRODUCT_GROWTH_BASE
FROM [Car_MART]
GROUP BY brand, model

-- 세션 임시 테이블(#PRODUCT_GROWTH_BASE) 조회
SELECT *
FROM #PRODUCT_GROWTH_BASE
```
- 브랜드(brand) 및 모델(model)별 2020, 2021년 구매금액이 세션 임시 테이블로 생성.

### 1-5-2 #PRODUCT_GROWTH_BASE 테이블을 활용한 브랜드별 성장률 파악

```sql
SELECT brand,
       SUM(tot_amt_2021) / SUM(tot_amt_2020) -1 AS growth
FROM #PRODUCT_GROWTH_BASE
GROUP BY brand
ORDER BY 2 DESC
```
- 엑셀로 분석한 것과 결과가 같음.(그래프 제외, 결과만)

![image](https://user-images.githubusercontent.com/75361137/155080979-8a7a5298-8fc7-4853-ad58-3b30118a5603.png)

### 1-5-3 #PRODUCT_GROWTH_BASE 테이블을 활용한 브랜드 및 모델별 성장률 파악
- 조건 : 각 브랜드별 성장률 TOP 2 모델만 필터

```sql
-- 브랜드 및 모델별 성장률 분석(+각 브랜드별 모델 성장률 순위)
SELECT *,
       ROW_NUMBER() OVER(PARTITION BY brand ORDER BY growth DESC) AS rnk
FROM (
    SELECT brand, model,
           SUM(tot_amt_2021) / SUM(tot_amt_2020) -1 AS growth
    FROM #PRODUCT_GROWTH_BASE
    GROUP BY brand, model
    ) A
```
- 브랜드(brand) 및 모델(model)별 성장률 및 브랜드별 모델 성장률 순위 조회.
- ROW_NUMBER()을 통해 브랜드별 모델 성장률 순위를 반환.

- 위의 쿼리를 서브쿼리로 하여 모델 성장률 순위(rnk) 2 이하만 필터.
```sql
-- 브랜드 및 모델별 성장률 분석(+각 브랜드별 모델 성장률 순위)
-- 조건 : 각 브랜드별 성장률 TOP 2 모델만 필터링
SELECT *
FROM (
    SELECT *,
           ROW_NUMBER() OVER(PARTITION BY brand ORDER BY growth DESC) AS rnk
    FROM (
        SELECT brand, model,
               SUM(tot_amt_2021) / SUM(tot_amt_2020) -1 AS growth
        FROM #PRODUCT_GROWTH_BASE
        GROUP BY brand, model
        ) A
    ) B
WHERE rnk <= 2 
```
- 엑셀로 분석한 것과 결과가 같음.(그래프 및 퍼센트 제외, 결과만)

![image](https://user-images.githubusercontent.com/75361137/155082096-3bc513f3-6438-433b-9062-d580ccbe3bf2.png)
