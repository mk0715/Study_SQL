# 1. 자주 쓰이는 SQL 문법

## 1-1 연산자
- 연산자는 주로 WHERE절에 사용되며, 종류는 비교 / 논리 / 특수 / 산술로 총 4가지.

### 1-1-1 비교 연산자

| 연산자 | 설명 | 
| ---------------- | ---------------- |
| = | 같음 |
| > | 보다 큼 |
| < | 보다 작음 |
| >= | 크거나 같음 |
| <= | 작거나 같음 |
| <> | 같지 않음 |

```sql
-- 1. 비교 연산자
-- [MEMBER] 테이블 사용
-- (1-1) [addr]이 'seoul'이 아닌 값만 조회하여라.
SELECT *
FROM [MEMBER]
WHERE addr <> 'seoul'
```

### 1-1-2 논리 연산자

| 연산자 | 설명 | 
| ---------------- | ---------------- |
| AND | 앞, 뒤 조건 모두 만족 |
| OR | 앞, 뒤 조건 중 하나라도 만족 |
| NOT | 뒤에 오는 조건과 반대 |

```sql
-- 2. 논리 연산자
-- [MEMBER] 테이블 사용
-- (2-1) [gender]가 'man' 및 [ageband]가 20인 값만 조회하여라.
SELECT *
FROM [MEMBER]
WHERE gender = 'man' AND ageband = 20

-- (2-2) [gender]가 'man' 및 [ageband]가 20인 값과 또는 [addr]이 'seoul'인 값을 조회하여라.
SELECT *
FROM [MEMBER]
WHERE (gender = 'man' AND ageband = 20) OR addr = 'seoul'
```

### 1-1-3 특수 연산자

| 연산자 | 설명 | 
| ---------------- | ---------------- |
| BETWEEN a AND b | a와 b의 값 사이 |
| NOT BETWEEN a AND b | a와 b의 값 사이가 아님 |
| IN(List) | 리스트(List) 값 |
| NOT IN(List) | 리스트(List) 값이 아님 |
| LIKE '비교문자열' | 비교문자열 형태 EX) '모%', '%모%', '%모' |
| NOT LIKE '비교문자열' | 비교문자열 형태가 아님 |
| IS NULL | NULL 값 |
| IS NOT NULL | NULL 값이 아님 |

```sql
-- 3. 특수 연산자
-- [MEMBER] 테이블 사용
-- (3-1) [ageband]가 20~40 사이 값만 조회하여라.
SELECT *
FROM [MEMBER]
WHERE ageband BETWEEN 20 AND 40

-- (3-2) [addr]이 'ae'를 포함하는 값만 조회하여라.
SELECT *
FROM [MEMBER]
WHERE addr LIKE '%ae%'
```

### 1-1-4 산술 연산자

| 연산자 | 설명 | 
| ---------------- | ---------------- |
| + | 덧셈 |
| - | 뺄셈 |
| * | 곱셈 |
| / | 나눗셈 |


```sql
-- 4. 산술 연산자
-- [ORDER] 테이블 사용
-- (4-1) [sales_amt]을 0.1을 곱셈하여라.(컬럼명은 fees)
SELECT *, sales_amt * 0.1 AS fees
FROM [ORDER]

-- (4-2) [sales_amt]에 [sales_amt]을 0.1을 곱셈한 값을 뺄셈하여라.(컬럼명은 Excluding_fees)
SELECT *, sales_amt * 0.1 AS fees, sales_amt - (sales_amt * 0.1) AS Excluding_fees
FROM [ORDER]
```

## 1-2 단일 행 함수
- 함수는 어떤 입력값이 규칙에 의해 새로운 결괏값을 반환하는 것이며, SQL 함수는 단일 및 복수행 2가지 함수 유형으로 분류.
- 그 중 단일 행 함수는 단일 행을 대상으로 함수를 적용.
- 단일 행 함수는 숫자 / 문자 / 날짜 / 형 변환 / 일반 총 5가지 종류.

### 1-2-1 숫자형 함수

| 함수 | 설명 | 함수 사용 | 결과 |
| ---------------- | ---------------- | ---------------- | ---------------- |
| ABS(숫자) | 절댓값 반환 | SELECT ABS(-123) | 123 |
| ROUND(숫자, m) | 소숫점 m 자리로 반올림 | SELECT ROUND(1.56, 1) | 1.60 |
| CEILING(숫자) | 크거나 같은 최소 정수 | SELECT CEILING(1.56) | 2 |
| FLOOR(숫자) | 작거나 같은 최소 정수 | SELECT FLOOR(1.56) | 1 |
| POWER(n, m) | n의 m 거듭 제곱 | SELECT POWER(3,2) | 9 |
| SQRT(n) | n의 제곱근 | SELECT SQRT(9) | 3 |

### 1-2-2 문자형 함수

| 함수 | 설명 | 함수 사용 | 결과 |
| ---------------- | ---------------- | ---------------- | ---------------- |
| LOWER('문자열') | 소문자 반환 | SELECT LOWER('ABCD') | abcd |
| UPPER('문자열') | 대문자 반환 | SELECT UPPER('abcd') | ABCD |
| LEFT('문자열', n) | 왼쪽부터 지정한 수(n)만큼 문자 반환 | SELECT LEFT('ABCD', 2) | AB |
| RIGHT('문자열', n) | 오른쪽부터 지정한 수(n)만큼 문자 반환 | SELECT RIGHT('ABCD', 2) | CD |
| LEN('문자열') | 문자수 반환(오른쪽 공백 제외) | SELECT LEN('ABCD') | 4 |
| REPLACE('문자열', a, b) | 문자열 중, a가 있으면 b로 반환 | SELECT REPLACE('ABCD', 'A', 'a') | aBCD |
| CONCAT('문자열', '문자열', ..) | 둘 이상의 문자열 연결 | SELECT CONCAT('AB', 'C', 'D') | ABCD |
| LTRIM('문자열') | 왼쪽 공백 제외 문자 반환 | SELECT LTRIM(' ABCD') | ABCD |
| RTRIM('문자열') | 오른쪽 공백 제외 문자 반환 | SELECT RTRIM('ABCD ') | ABCD |
| TRIM('문자열') | 양쪽 공백 제외 문자 반환 | SELECT TRIM(' ABCD ') | ABCD |
| SUBSTRING('문자열', m, n) | 문자열의 m위치에서 n개 길이만큼 문자 반환 | SELECT SUBSTRING('ABCD', 2, 2) | BC |
| CHARINDEX(a, '문자열', n) | 문자열 중, a가 있으면 위치 반환(a가 없으면 0 반환), n으로 검색 시작 위치 지정 | SELECT CHARINDEX('B', 'ABCD') </br>SELECT CHARINDEX('B', 'ABCD', 3) | 2</br>0 |
| SPACE(n) | n만큼 공백 추가 | SELECT 'A'+SPACE(5)+'B' | A     B |
| STUFF('문자열', m, n, a) | 문자열의 m 위치에서 n개 길이만큼 삭제 후, a로 문자 반환 | SELECT STUFF('ABCD', 2, 2 'xx') | AxxD |

### 1-2-3 날짜형 함수

| 함수 | 설명 | 함수 사용 | 결과 |
| ---------------- | ---------------- | ---------------- | ---------------- |
| GET-DATE() | 현재 날짜 및 시간 반환 | SELECT GETDATE() | 현재 날짜/시간 |
| YEAR / MONTH / DAY(날짜열) | YEAR, MONTH, DAY 데이터 반환 | SELECT YEAR('2020-12-31') </br> SELECT MONTH('2020-12-31') </br> SELECT DAY('2020-12-31') | 2020 </br> 12 </br> 31 |
| DATE-PART(기준, 날짜열) | 지정 기준으로, 데이터 반환 | SELECT DATEPART(YEAR, '2020-12-31 10:20:30') </br> SELECT DATEPART(MONTH, '2020-12-31 10:20:30') </br> SELECT DATEPART(DAY, '2020-12-31 10:20:30') </br> SELECT DATEPART(HH, '2020-12-31 10:20:30') </br> SELECT DATEPART(MI, '2020-12-31 10:20:30') </br> SELECT DATEPART(SS, '2020-12-31 10:20:30') </br> SELECT DATEPART(DW, '2020-12-31 10:20:30') </br> *DW : 1~7 -> 일요일~토요일 | 2020 </br> 12 </br> 31 </br> 10 </br> 20 </br> 30 </br> 5 -> 목 |
| DATEADD(기준, n, 날짜열) | 지정한 기준에서, n 숫자만큼 더한 데이터 반환 | SELECT DATEADD(YEAR, -1, '2020-12-31') </br> SELECT DATEADD(MONTH, -1, '2020-12-31') </br> SELECT DATEADD(DAY, -1, '2020-12-31') | 2019-12-31 00:00:00.000 </br> 2020-11-30 00:00:00.000 </br> 2020-12-30 00:00:00.000 |
| DATE-DIFF(기준, 날짜열a, 날짜열b) | 지정한 기준으로 날짜열a, b 차이 데이터 반환 | SELECT DATEDIFF(YEAR, '2020-12-31', '2021-12-31') </br> SELECT DATEDIFF(MONTH, '2020-12-31', '2021-12-31') </br> SELECT DATEDIFF(DAY, '2020-12-31', '2021-12-31') </br> SELECT DATEDIFF(HH, '2020-12-31', '2021-12-31') </br> SELECT DATEDIFF(MI, '2020-12-31', '2021-12-31') </br> SELECT DATEDIFF(SS, '2020-12-31', '2021-12-31') | 1 </br> 12 </br> 365 </br> 8760 </br> 525600 </br> 31536000 |

### 1-2-4 형 변환 함수

| 함수 | 설명 | 함수 사용 | 결과 |
| ---------------- | ---------------- | ---------------- | ---------------- |
| CONVERT(VARCHAR, '날짜열', 기준) | 지정 기준으로 날짜 -> 문자열 데이터 형식 변환 | SELECT CONVERT(VARCHAR, GETDATE(), 1) </br> SELECT CONVERT(VARCHAR, GETDATE(), 5) </br> SELECT CONVERT(VARCHAR, GETDATE(), 8) </br> SELECT CONVERT(VARCHAR, GETDATE(), 21) </br> SELECT CONVERT(VARCHAR, GETDATE(), 23) </br> SELECT CONVERT(VARCHAR, GETDATE(), 111) </br> SELECT CONVERT(VARCHAR, GETDATE(), 112) </br> SELECT CONVERT(VARCHAR, GETDATE(), 120) | 03/03/20 </br> 03-03-20 </br> 17:05:30 </br> 2020-03-03 17:05:30.007 </br> 2020-03-03 </br> 2020/03/03 </br> 20200303 </br> 2020-03-03 17:05:30 |
| CONVERT(VARCHAR(n), '날짜열', 기준) | 지정 기준으로 날짜 -> 문자열 데이터 형식 변환 * n 숫자만큼 | SELECT CONVERT(VARCHAR(8), GETDATE(), 112) </br> SELECT CONVERT(VARCHAR(6), GETDATE(), 112) </br> SELECT CONVERT(VARCHAR(4), GETDATE(), 112) | 20200303 </br> 202003 </br> 2020 |
| CAST(특정 열 AS 기준) | 지정 기준으로 특정 열 데이터 형식 변환 | SELECT CAST('100' AS INT) + CAST('100' AS INT) </br> SELECT CAST('100' AS VARCHAR) + CAST('100' AS VARCHAR) </br> SELECT CAST(1.56 AS INT) </br> SELECT CAST(1.56 AS NUMERIC) </br> SELECT CAST(GETDATE() AS DATE) </br> SELECT CAST(GETDATE() AS DATETIME)  | 200 </br> 100100 </br> 1 </br> 2 </br> 2020-03-03 </br> 2020-03-03 17:05:30.007 |


### 1-2-5 일반 함수

| 함수 | 설명 | 함수 사용 | 결과 |
| ---------------- | ---------------- | ---------------- | ---------------- |
| ISNULL(특정 열, 지정 값) | 특정 열이 NULL일 경우, 지정 값 반환 | SELECT ISNULL(NULL, 0) | 0 |
| NULLIF(특정 열a, 특정 열b) | 특정 열 a,b가 같은 경우 NULL / 다를 경우 특정열a 변환 | SELECT NULLIF(1, 1) </br> SELECT NULLIF(1, 2) | NULL </br> 1 |
| CASE WHEN [조건1] THEN [지정 값1] </br> 　　 WHEN [조건2] THEN [지정 값2] </br> 　　  END  | 여러 조건별로 지정 값으로 변환 </br> * 조건 외 값은 NULL 반환 | 아래 SQL 구문 참조 | - |

```sql
-- CASE WHEN : 여러 조건별로 지정 값 변환
-- 조건 외 값은 NULL 반환
SELECT *, CASE WHEN ageband BETWEEN 20 AND 30 THEN '2030세대'
               WHEN ageband BETWEEN 40 AND 50 THEN '4050세대'
               END AS ageband_seg
FROM [MEMBER]

-- CASE WHEN(+ELSE) : 여러 조건별로 지정 값 변환(+ELSE)
-- 조건 외 값을 ELSE로 지정
SELECT *, CASE WHEN ageband BETWEEN 20 AND 30 THEN '2030세대'
               WHEN ageband BETWEEN 40 AND 50 THEN '4050세대'
               ELSE 'OTHER세대' END AS ageband_seg
FROM [MEMBER]
```


## 1-3 복수 행 함수
- 복수 행 함수는 복수 행을 그룹화하여 함수를 적용.
- 집계 및 그룹 함수 총 2가지 종류.

### 1-3-1 집계 함수(NULL 값 제외)

| 함수 | 설명 | 
| ---------------- | ---------------- |
| COUNT | 건수 ** COUNT(*)은 NULL 값을 포함한 건수 |
| SUM | 합계값 |
| AVG | 평균값 |
| MAX | 최고값 |
| MIN | 최소값 |
| STDEV | 표준 편차값 |
| VAR | 분산값 |

- 집계 함수는 테이블의 전체 또는 여러 행에 대한 하나의 결괏값을 반환.
- 집계 함수는 테이블의 전체 행, 집계 함수 + GROUP BY는 테이블의 여러 행.

```sql
-- 집계 함수
SELECT COUNT(order_no) AS 주문수,
       SUM(sales_amt) AS 주문금액,
       AVG(sales_amt) AS 평균주문금액,
       MAX(order_date) AS 최근구매일자,
       MIN(order_date) AS 최초구매일자,
       STDEV(sales_amt) AS 주문금액_표준편차,
       VAR(sales_amt) AS 주문금액_분산
FROM [ORDER]
```
- [ORDER] 테이블 전체 행에 대한 집계 함수 조회.

```sql
-- 집계 함수 + GROUP BY
SELECT mem_no,
       COUNT(order_no) AS 주문수,
       SUM(sales_amt) AS 주문금액,
       AVG(sales_amt) AS 평균주문금액,
       MAX(order_date) AS 최근구매일자,
       MIN(order_date) AS 최초구매일자,
       STDEV(sales_amt) AS 주문금액_표준편차,
       VAR(sales_amt) AS 주문금액_분산
FROM [ORDER]
GROUP BY mem_no
```
- [ORDER] 테이블의 [mem_no]열을 그룹으로 묶은 여러 행에 대한 집계 함수 조회.


### 1-3-2 그룹 함수
- 그룹 함수는 GROUP BY의 항목들을 그룹으로 묶는 함수.

| 함수 | 설명 | 
| ---------------- | ---------------- |
| WITH ROLLUP | GROUP BY 항목들을 오른쪽에서 왼쪽 순으로 그룹 |
| WITH CUBE | GROUP BY 항목들의 모든 경우의 수를 그룹 |
| GROUPING SET | GROUP BY 항목들을 개별 그룹 |
| GROUPING | WITH ROLLUP 및 CUBE에 의해 그룹화 되었다면 0, 그렇지 않으면 1을 반환 |

```sql
-- WITH ROLLUP
-- GROUP BY 항목들을 오른쪽에서 왼쪽 순으로 그룹으로 묶음
SELECT YEAR(order_date) AS 연도,
       channel_code AS 채널코드,
       SUM(sales_amt) AS 주문금액
FROM [ORDER]
GROUP BY YEAR(order_date), channel_code
WITH ROLLUP
ORDER BY 1 DESC, 2 ASC
```
- WITH ROLLUP은 GROUP BY 항목들의 오른쪽에서 왼쪽 순으로 그룹으로 묶음.
- YEAR은 날짜 형식의 데이터를 연도로 변환.
- WITH ROLLUP은 GROUP BY 다음에 작성해야 함.

```sql
-- WITH CUBE
-- GROUP BY 항목들의 모든 경우의 수를 그룹으로 묶음
SELECT YEAR(order_date) AS 연도,
       channel_code AS 채널코드,
       SUM(sales_amt) AS 주문금액
FROM [ORDER]
GROUP BY YEAR(order_date), channel_code
WITH CUBE
ORDER BY 1 DESC, 2 ASC
```
- WITH CUBE는 GROUP BY 항목들의 모든 경우의 수를 묶음.
- WITH CUBE는 GROUP BY 다음에 작성해야 함.

```sql
-- GROUPING SETS
-- GROUP BY 항목들을 개별 그룹으로 묶음
SELECT YEAR(order_date) AS 연도,
       channel_code AS 채널코드,
       SUM(sales_amt) AS 주문금액
FROM [ORDER]
GROUP BY GROUPING SETS (YEAR(order_date), channel_code)
```
- GROUPING SETS는 GROUP BY 항목들을 개별 그룹으로 묶음. -> 각 항목에 대한 집계 값이 조회.
- GROUPING SETS는 GROUP BY와 그 항목들 사이에 작성하여 항목들을 ()로 묶어야 함.

```sql
-- GROUPING
-- GROUP BY 항목들을 개별 그룹으로 묶음
SELECT YEAR(order_date) AS 연도,
       GROUPING(YEAR(order_date)) AS 연도_GROUPING,
       channel_code AS 채널코드,
       GROUPING(channel_code) AS 채널코드_GROUPING,
       SUM(sales_amt) AS 주문금액
FROM [ORDER]
GROUP BY YEAR(order_date), channel_code
WITH ROLLUP
ORDER BY 1 DESC, 2 ASC
```
- GROUPING은 GROUP BY 항목들을 ()로 묶어 작성해야 함.

```sql
-- GROUPING 및 CASE WHEN을 활용한 총계 및 소계 변환

-- (1) YEAR(order_date) 및 channel_code 숫자 -> 문자 형 변환
SELECT CAST(YEAR(order_date) AS VARCHAR) AS 연도,
       CAST(channel_code AS VARCHAR) AS 채널코드,
       sales_amt
FROM [ORDER]

-- (2) (1)을 서브 쿼리로 하여, CASE WHEN으로 총계 및 소계 변환
SELECT CASE WHEN GROUPING(연도)=1 THEN '총계'
            ELSE 연도 END AS 연도_총계,

       CASE WHEN GROUPING(연도)=1 THEN '총계'
            WHEN GROUPING(채널코드)=1 THEN '소계'
            ELSE 채널코드 END AS 채널코드_총소계,

       SUM(sales_amt) AS 주문금액
FROM (
    SELECT CAST(YEAR(order_date) AS VARCHAR) AS 연도,
            CAST(channel_code AS VARCHAR) AS 채널코드,
            sales_amt
    FROM [ORDER]
    ) A
GROUP BY 연도, 채널코드
WITH ROLLUP
ORDER BY 1 DESC, 2 ASC
```
- WITH ROLLUP에 의해 그룹화된 값을 GROUPING 및 CASE WHEN을 활용하여 총계 및 소계를 표기.


## 1-4 윈도우 함수
- 윈도우 함수는 행과 행간의 관계를 정의하며, 순위 및 집계 함수(누적) 총 2가지 종류.
- 기본 문법은 윈도우 함수 + OVER(ORDER BY 열 ASC or DESC)이며, PARTITION BY는 옵션.

### 1-4-1 순위 함수
- 순위 함수는 ORDER BY(+PARTITION BY) 열 기준으로 행과 행간의 순위를 반환.

| 함수 | 설명 | 
| ---------------- | ---------------- |
| ROW_NUMBER() | 동일한 값에 대해 고유한 순위 반환 |
| RANK() | 동일한 값에 대해 고유한 순위 반환(ex. 1위, 2위, 2위, 4위..) |
| DENSE_RANK() | RANK()와 흡사하나, 동일한 순위를 하나의 등수로 간주(ex. 1위, 2위, 3위, 4위..) |

```sql
-- 순위 함수
-- ORDER BY
SELECT order_date,
       ROW_NUMBER() OVER (ORDER BY order_date ASC) AS ROW_NUMBER,
       RANK() OVER (ORDER BY order_date ASC) AS RANK,
       DENSE_RANK() OVER (ORDER BY order_date ASC) AS DENSE_RANK
FROM [ORDER]
```
- order_date 열 기준 오름차순으로 순위 조회.
- ROW_NUMBER() : 고유한 순위 반환(1,2,3,4..)
- RANK() : 동일한 순위 반환(1,1,1,4..)
- DENSE_RANK() : 동일한 순위 반환(*하나의 등수로 간주 : 1,1,1,3..)

```sql
-- ORDER BY + PARTITION BY
SELECT mem_no, order_date,
       ROW_NUMBER() OVER (PARTITION BY mem_no ORDER BY order_date ASC) AS ROW_NUMBER,
       RANK() OVER (PARTITION BY mem_no ORDER BY order_date ASC) AS RANK,
       DENSE_RANK() OVER (PARTITION BY mem_no ORDER BY order_date ASC) AS DENSE_RANK
FROM [ORDER]
```
- order_date 열 기준 오름차순으로 순위가 조회되고, PARTITION BY로 인해 mem_no 열별로 구분되어 조회.


### 1-4-2 집계 함수(누적)
- 집계 함수(누적)는 ORDER BY(+PARTITION BY) 열 기준으로 행과 행간의 누적 집계를 반환.

| 함수 | 설명 | 
| ---------------- | ---------------- |
| COUNT() | 누적 건수 |
| SUM() | 누적 합계값 |
| AVG() | 누적 평균값 |
| MAX() | 누적 최고값 |
| MIN() | 누적 최소값 |

```sql
-- 집계 함수
-- ORDER BY
SELECT order_date,
       sales_amt,
       COUNT(sales_amt) OVER (ORDER BY order_date ASC) AS 구매횟수,
       SUM(sales_amt) OVER (ORDER BY order_date ASC) AS 구매금액,
       AVG(sales_amt) OVER (ORDER BY order_date ASC) AS 평균구매금액,
       MAX(sales_amt) OVER (ORDER BY order_date ASC) AS 가장높은구매금액,
       MIN(sales_amt) OVER (ORDER BY order_date ASC) AS 가장낮은구매금액
FROM [ORDER]
```
- order_date 열 기준 오름차순으로 누적 집계가 반환되고, 동일 날짜에 대한 sales_amt 누적 집계 방식이 함수에 따라 다름.

```sql
-- ORDER BY + PARTITION BY
SELECT mem_no, order_date, sales_amt,
       COUNT(sales_amt) OVER (PARTITION BY mem_no ORDER BY order_date ASC) AS 누적_구매횟수,
       SUM(sales_amt) OVER (PARTITION BY mem_no ORDER BY order_date ASC) AS 누적_구매금액,
       AVG(sales_amt) OVER (PARTITION BY mem_no ORDER BY order_date ASC) AS 누적_평균구매금액,
       MAX(sales_amt) OVER (PARTITION BY mem_no ORDER BY order_date ASC) AS 누적_가장높은구매금액,
       MIN(sales_amt) OVER (PARTITION BY mem_no ORDER BY order_date ASC) AS 누적_가장낮은구매금액
FROM [ORDER]
```
- order_date 열 기준 오름차순으로 누적 집계가 반환되고, PARTITION BY로 인해 mem_no 열별로 구분되어 누적 집계 조회.


## 1-5 집합 연산자
- 집합 연산자는 두 개 이상의 SELECT절의 결과를 하나로 결합하며, 총 4가지 종류.

| 연산자 | 집합 | 설명 | 
| ---------------- | ---------------- | ---------------- |
| UNION | 합집합 | 중복된 행을 하나의 행으로 반환 |
| UNION ALL | 합집합 | 중복된 행을 그대로 반환 |
| INTERSECT | 교집합 | 중복된 행은 하나의 행으로 반환 |
| EXCEPT | 차집합 | 중복된 행은 하나의 행으로 반환 |

### 1-5-1 UNION vs UNION ALL
![image](https://user-images.githubusercontent.com/75361137/154905268-9c76519a-5f52-4f7b-ba5c-df0ec011b600.png)
- UNION 및 UNION ALL은 두 테이블을 합집합 형태로 결합하는 연산자.
- UNION : 중복된 행을 하나의 행으로 반환.
- UNION ALL : 중복된 행을 그대로 반환.

```sql
-- UNION : 두 테이블을 합집합 형태로 결과 반환
-- 중복된 행을 하나의 행으로 반환(한번만 나옴)
SELECT *
FROM [MEMBER_1]
UNION
SELECT *
FROM [MEMBER_2]


-- UNION ALL : 두 테이블을 합집합 형태로 결과 반환
-- 중복된 행을 그대로 반환(두번 다 나옴)
SELECT *
FROM [MEMBER_1]
UNION ALL
SELECT * 
FROM [MEMBER_2]
```


### 1-5-2 INTERSECT vs EXCEPT
![image](https://user-images.githubusercontent.com/75361137/154905399-365df0b6-976e-4843-9778-56f24e7ed67d.png)
- INTERSECT는 교집합, EXCEPT는 차집합 형태로 결합하는 연산자.
- INTERSECT 및 EXCEPT : 중복된 행은 하나의 행으로 반환(한번만 나옴)

```sql
-- INTERSECT : 두 테이블을 교집합 형태로 결과 반환
-- 중복된 행을 하나의 행으로 반환
SELECT *
FROM [MEMBER_1]
INTERSECT
SELECT *
FROM [MEMBER_2]


-- EXCEPT : 두 테이블을 차집합 형태로 결과 반환
-- 중복된 행을 하나의 행으로 반환
SELECT *
FROM [MEMBER_1]
EXCEPT
SELECT *
FROM [MEMBER_2]
```


# 2. 효율화 & 자동화에 필요한 SQL 문법

## 2-1 VIEW
- VIEW(뷰)는 가상 테이블 또는 저장된 SQL 명령어.
- 자주 사용되는 SQL 명령어를 저장함으로써 효율적으로 SQL 명령어 실행.

### 2-1-1 VIEW 생성

```sql
-- VIEW
-- 생성
CREATE VIEW [ORDER_MEMBER]
AS
SELECT A.*,
       B.gender,
       B.ageband,
       B.join_date
FROM [ORDER] A
LEFT JOIN [MEMBER] B
ON A.mem_no = B.mem_no
```
- mem_no열은 ORDER 테이블과 MEMBER 테이블 모두에 있어서 중복되는 열 이름을 조회하는 SQL 명령어는 VIEW로 생성할 수 없기 때문에 제외.

### 2-1-2 VIEW 조회

```sql
-- VIEW
-- 조회
SELECT * 
FROM [ORDER_MEMBER]
```
- ORDER_MEMBER에 저장된 SQL 명령어 조회.

### 2-1-3 VIEW 수정

```sql
-- VIEW
-- 수정
ALTER VIEW [ORDER_MEMBER]
AS
SELECT A.*,
       B.gender,
       B.ageband,
       B.join_date
FROM [ORDER] A
LEFT JOIN [MEMBER] B
ON A.mem_no = B.mem_no
WHERE A.channel_code = 1
```
- ALTER 명령어로 WHERE 절 추가.

### 2-1-4 VIEW 삭제

```sql
-- VIEW
-- 삭제
DROP VIEW [ORDER_MEMBER]
```


## 2-2 PROCEDURE
- PROCEDURE(프로시저)는 VIEW(뷰)와 같이 SQL 명령어를 저장하지만, 매개 변수라는 프로그래밍 기능이 추가.
- 자주 사용하는 SQL 명령어를 저장 및 매개 변수를 활용함으로써 자동적으로 실행 가능.

### 2-2-1 PROCEDURE 생성

```sql
-- PROCEDURE
-- 생성
CREATE PROCEDURE [ORDER_MEMBER]
(
@channel_code AS INT
)
AS
SELECT *
FROM [ORDER] A
LEFT JOIN [MEMBER] B
ON A.mem_no = B.mem_no
WHERE A.channel_code = @channel_code
```
- @매개 변수는 데이터 형식을 지정
- channel_code를 숫자로 지정하면 그에 맞는 데이터 조회
- VIEW와는 다르게 열 이름이 중복되어도 모든 열을 조회할 수 있는 것은 PROCEDURE은 가상 테이블이 아니기 때문.

### 2-2-2 PROCEDURE 실행

```sql
-- PROCEDURE
-- 실행
EXEC [ORDER_MEMBER] 3
```
- 변수를 3으로 입력했기에 channel_code = 3으로 필터되어 조회.

### 2-2-3 PROCEDURE 수정

```sql
-- PROCEDURE
-- 수정
ALTER PROCEDURE [ORDER_MEMBER]
(
@channel_code AS INT,
@YEAR_order_date AS INT
)
AS
SELECT *
FROM [ORDER] A
LEFT JOIN [MEMBER] B
ON A.mem_no = B.mem_no
WHERE A.channel_code = @channel_code
AND YEAR(order_date) = @YEAR_order_date
```
- 매개변수 및 WHERE절 추가.

### 2-2-4 PROCEDURE 삭제
```sql
-- PROCEDURE
-- 삭제
DROP PROCEDURE [ORDER_MEMBER]
```


# 3. 정리

## 3-1 데이터 마트
- 데이터 마트(Data Mart)는 분석 목적에 맞게 데이터를 가공한 분석용 데이터 세트.

```sql
-- 데이터 마트 생성 예시
-- 분석 목적 : 2020년도 주문금액 및 건수를 회원 프로파일 별로 확인하고자 한다.

-- 1. [ORDER] 테이블의 [mem_no]별 [sales_amt] 합계 및 [order_no] 개수
-- * 조건 : [order_date]는 2020년
-- * 열 이름 : [sales_amt] 합계는 tot_amt / [order_no] 개수는 tot_tr
SELECT mem_no,
       SUM(sales_amt) AS tot_amt,
       COUNT(order_no) AS tot_tr
FROM [ORDER]
WHERE YEAR(order_date) = 2020
GROUP BY mem_no


-- 2. [MEMBER] 테이블을 왼쪽으로 하여 (1) 테이블 LEFT JOIN
SELECT A.*,
       B.tot_amt,
       B.tot_tr
FROM [MEMBER] A 
LEFT JOIN(
     SELECT mem_no,
            SUM(sales_amt) AS tot_amt,
            COUNT(order_no) AS tot_tr
     FROM [ORDER]
     WHERE YEAR(order_date) = 2020
     GROUP BY mem_no 
) B
ON A.mem_no = B.mem_no


-- 3. (2)를 활용하여 구매여부 열 추가
SELECT A.*,
       B.tot_amt,
       B.tot_tr,
       CASE WHEN B.mem_no IS NOT NULL THEN '구매자'
            ELSE '미구매자' END AS pur_yn
FROM [MEMBER] A 
LEFT JOIN(
     SELECT mem_no,
            SUM(sales_amt) AS tot_amt,
            COUNT(order_no) AS tot_tr
     FROM [ORDER]
     WHERE YEAR(order_date) = 2020
     GROUP BY mem_no 
) B
ON A.mem_no = B.mem_no


-- 4. (3)의 조회된 결과 테이블 생성
SELECT A.*,
       B.tot_amt,
       B.tot_tr,
       CASE WHEN B.mem_no IS NOT NULL THEN '구매자'
            ELSE '미구매자' END AS pur_yn
INTO [MART_2020] -- INTO[테이블명]으로 새로운 테이블 생성
FROM [MEMBER] A 
LEFT JOIN(
     SELECT mem_no,
            SUM(sales_amt) AS tot_amt,
            COUNT(order_no) AS tot_tr
     FROM [ORDER]
     WHERE YEAR(order_date) = 2020
     GROUP BY mem_no 
) B
ON A.mem_no = B.mem_no
```


## 3-2 데이터 정합성
- 데이터 정합성은 분석 데이터 값들이 서로 일관되게 일치함을 나타내는 정도.

```sql
-- 데이터 마트[MART_2020] 정합성 체크

-- 1. [MART_2020] 데이터 마트의 회원수 중복은 없는가?
SELECT COUNT(mem_no) AS 회원수,
       COUNT(DISTINCT mem_no) AS 회원수_중복제거
FROM [MART_2020]

-- 2. [MEMBER] 테이블과 [MART_2020] 데이터 마트의 회원 수 차이는 없는가?
SELECT COUNT(mem_no) AS 회원수,
       COUNT(DISTINCT mem_no) AS 회원수_중복제거
FROM [MEMBER]

-- 3. [ORDER] 테이블과 [MART_2020] 데이터 마트의 주문수 차이는 없는가?
SELECT SUM(tot_tr) AS 주문수
FROM [MART_2020]

SELECT COUNT(order_no) AS 주문수,
       COUNT(DISTINCT order_no) AS 주문수_중복제거
FROM [ORDER]
WHERE YEAR(order_date) = 2020

-- 4. [MART_2020] 데이터 마트의 미구매자는 [ORDER] 테이블에서 2020년에 구매가 없는가?
SELECT *
FROM [ORDER]
WHERE mem_no IN (SELECT mem_no FROM [MART_2020] WHERE pur_yn = '미구매자')
AND YEAR(order_date) = 2020
```
- DISTINCT는 중복된 값을 제거하고 고유한 값만 반환.

