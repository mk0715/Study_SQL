# 1. 조회(SELECT)

## 1-1 데이터 조회(SELECT)와 함께 사용되는 절들
- SELECT는 데이터 조작 언어(DML)중 하나이자 데이터 분석에 있어서 가장 많이 사용되는 명령어.
- SELECT와 함께 사용되는 절은 FROM / WHERE / GROUP BY / HAVING / ORDER BY 등이 존재.

### 1-1-1 SELECT + FROM
```sql
USE EDU

-- FROM : [MEMBER] 테이블 선택
SELECT *
FROM [MEMBER]
```
- FROM절은 테이블을 선택하는 명령어.

### 1-1-2 SELECT + FROM + WHERE
```sql
-- WHERE : gender 컬럼값이 'man'으로만 필터링
SELECT *
FROM [MEMBER]
WHERE gender = 'man'
```
- WHERE는 테이블을 특정 조건으로 필터하는 명령어.

### 1-1-3 SELECT + FROM + WHERE + GROUP BY
```sql
-- GROUP BY : addr 컬럼별로 회원(mem_no)수 집계
SELECT addr, COUNT(mem_no) AS [회원수집계]
FROM [MEMBER]
WHERE gender = 'man'
GROUP BY addr

-- GROUP BY : addr, gender 컬럼별로 회원(mem_no)수 집계
SELECT addr, gender, COUNT(mem_no) AS [회원수집계]
FROM [MEMBER]
WHERE gender = 'man'
GROUP BY addr, gender
```
- GROUP BY는 특정 열들을 그룹으로 묶는 명령어.
- COUNT는 특정 조건을 만족하는 열의 개수를 구하는 집계 함수. -> 후에 설명.

### 1-1-4 SELECT + FROM + WHERE + GROUP BY + HAVING
```sql
-- HAVING : addr 컬럼별로 회원(mem_no)수가 50이상만 필터링
SELECT addr, COUNT(mem_no) AS [회원수집계]
FROM [MEMBER]
WHERE gender = 'man'
GROUP BY addr
HAVING COUNT(mem_no) >= 50
```
- HAVING은 GROUP BY된 테이블을 특정 조건으로 필터하는 명령어.
- WHERE과 차이점으로는 WHERE은 [FROM + 테이블]로 만들어진 테이블의 행을 필터, HAVING은 [GROUP BY+열]로 만들어진 새로운 테이블의 행을 필터.

### 1-1-5 SELECT + FROM + WHERE + GROUP BY + HAVING + ORDER BY
```sql
-- ORDER BY : addr 컬럼별로 회원(mem_no)수가 높은 순(DESC)으로 정렬
SELECT addr, COUNT(mem_no) AS [회원수집계]
FROM [MEMBER]
WHERE gender = 'man'
GROUP BY addr
HAVING COUNT(mem_no) >= 50
ORDER BY COUNT(mem_no) DESC
```
- ORDER BY는 조회된 행을 정렬하는 명령어.
- DESC는 내림차순 정렬 옵션이며, ASC는 오름차순.

## 1-2 SQL 문법 vs 실행 순서
- SQL 문법 순서 : SELECT -> FROM -> WHERE -> GROUP BY -> HAVING -> ORDER BY
- SQL 실행 순서 : FROM -> WHERE -> GROUP BY -> HAVING -> SELECT -> ORDER BY로 SELECT만 실행 순서가 바뀜.

# 2. 결합(JOIN)

## 2-1 ERM이란?
- ERM(Entity-Relationship Modeling)은 데이터를 구조화하여 데이터베이스에 저장하기 위한 개체-관계 모델링 기법.

![image](https://user-images.githubusercontent.com/75361137/154421727-5bd64fc3-b232-430c-91c6-a05018dcd155.png)
- 위 그림은 개체(Entity)간의 관계(Relationship)를 모델링(Modeling)한 예시이며 한명의 회원이 여러 상품을 주문하는 관계 -> 1:N 관계.
- 회원은 N개의 상품을 주문할 수 있기 때문에, 회원 및 주문 테이블 관계는 1:N.
- 상품은 N번 주문될 수 있기 때문에, 주문 및 상품 테이블 관계는 N:1.
- 1:N 및 N:1 관계는 테이블 간의 관계를 설계할 때 사용되는 용어. 1은 중복 데이터가 아닌 고유한 PRIMARY KEY(PK)이며, N은 PK와 연결되는 중복 데이터가 가능한 FOREIGH KEY(FK).

![image](https://user-images.githubusercontent.com/75361137/154425063-8b3be682-9c7c-4779-85ff-a641238c1a99.png)
- 위의 그림에서 회원 테이블의 PK는 회원번호, 주문 테이블에서의 회원번호는 FK.
- 위의 그림에서 상품 테이블의 PK는 상품번호, 주문 테이블에서의 상품번호는 FK.

## 2-2 JOIN
- JOIN은 여러 테이블 간의 공통 값을 활용하여 테이블을 결합하는 명령어.
- JOIN의 종류는 INNER / OUTER / CROSS / SELF JOIN 총 4가지 종류가 존재.

### 2-2-1 INNER JOIN
![image](https://user-images.githubusercontent.com/75361137/154427943-8ef5ab4a-cdc2-4f7b-9c41-6ca20176f660.png)
- INNER JOIN은 두 테이블 간의 공통 값이 매칭되는 데이터만 조회하는 명령어.
```sql
USE EDU

-- INNER JOIN : 공통된 요소들을 통해 결합하는 조인 방식
-- [MEMBER] 및 [ORDER] 테이블 공통 값(mem_no) 결합
SELECT *
FROM [MEMBER] AS A -- [MEMBER] 테이블 별칭 A
INNER JOIN [ORDER] AS B -- [ORDER] 테이블 별칭 B
ON A.mem_no = B.mem_no -- 공통 값(mem_no) 결합
```
- AS(Aliases)는 임시로 별칭 생성.(생략 가능)
- ON은 두 테이블 간의 연결 조건을 지정하는 명령어. 대부분 PK 및 FK를 연결 조건으로 지정.

### 2-2-2 OUTER JOIN
![image](https://user-images.githubusercontent.com/75361137/154429031-b658682f-660f-4d92-a447-d61eefa00842.png)
![image](https://user-images.githubusercontent.com/75361137/154429064-266e9e6a-498d-48fe-a29c-b8875b4bb611.png)
![image](https://user-images.githubusercontent.com/75361137/154429096-7987f456-6da8-435a-8abc-742737a6f911.png)
- OUTER JOIN은 두 테이블 간의 공통 값이 매칭되는 데이터뿐만 아니라 매칭 안되는 데이터도 조회하는 명령어. 즉, 공통값이 매칭 안되는 데이터도 함께 조회할 때 사용되며 종류는 총 3가지(LEFT, RIGHT, FULL)
- LEFT JOIN : 왼쪽 테이블 기준 데이터 조회
- RIGHT JOIN : 오른쪽 테이블 기준 데이터 조회
- FULL JOIN : 양쪽 테이블 기준 데이터 조회

#### LEFT JOIN
```sql
-- LEFT JOIN : 왼쪽 테이블 기준 데이터 조회
-- [MEMBER] 및 [ORDER] 테이블 공통 값(mem_no) 결합 + 매칭 안되는 [MEMBER] 데이터 조회
SELECT *
FROM [MEMBER] A -- 왼쪽 테이블
LEFT JOIN [ORDER] B -- 오른쪽 테이블
ON A.mem_no = B.mem_no
```
- [MEMBER]과 [ORDER] 테이블 간의 공통 값(mem_no)이 매칭되는 데이터 뿐만 아니라 왼쪽 테이블 기준[MEMBER]으로 매칭 안 되는 데이터도 조회. -> 매칭 안되는 데이터는 모두 NULL값으로 조회.

#### RIGHT JOIN
```sql
-- RIGHT JOIN : 오른쪽 테이블 기준 데이터 조회
-- [MEMBER] 및 [ORDER] 테이블 공통 값(mem_no) 결합 + 매칭 안되는 [ORDER] 데이터 조회
SELECT *
FROM [MEMBER] A -- 왼쪽 테이블
RIGHT JOIN [ORDER] B -- 오른쪽 테이블
ON A.mem_no = B.mem_no
```
- [MEMBER]과 [ORDER] 테이블 간의 공통 값(mem_no)이 매칭되는 데이터 뿐만 아니라 오른쪽 테이블 기준[MEMBER]으로 매칭 안 되는 데이터도 조회. -> LEFT JOIN과 달리 회원가입 후에 주문이 가능하므로 NULL값이 조회되지 않음.

#### FULL JOIN
```sql
-- FULL JOIN : 양쪽 테이블 기준 데이터 조회
-- [MEMBER] 및 [ORDER] 테이블 공통 값(mem_no) 결합 + 매칭 안되는 [MEMBER] 및 [ORDER] 데이터 조회
SELECT *
FROM [MEMBER] A -- 왼쪽 테이블
FULL JOIN [ORDER] B -- 오른쪽 테이블
ON A.mem_no = B.mem_no
```
- [MEMBER]과 [ORDER] 테이블 간의 공통 값(mem_no)이 매칭되는 데이터 뿐만 아니라 양쪽 테이블 기준[MEMBER][ORDER]으로 매칭 안 되는 데이터도 조회.
- FULL JOIN은 LEFT 및 RIGHT JOIN 성질을 모두 가지고 있음.

### 2-2-3 OTHER(CROSS, SELF) JOIN
- CROSS JOIN은 두 테이블(A, B)의 행을 결합하는 명령어 -> A 테이블에 3개의 행이 있고 B 테이블에 N개의 행이 있으면 3 X N개의 행이 조회.
- SELF JOIN은 한 테이블(A)의 행과 다른 행들을 결합하는 명령어 -> A 테이블에 3개의 행이 있다면 3 X 3개의 행이 조회.

![image](https://user-images.githubusercontent.com/75361137/154433399-e12941ea-828f-4fb6-b53b-96630a823d8c.png)
![image](https://user-images.githubusercontent.com/75361137/154433428-af8dec39-a378-4e98-82bf-6649f3810346.png)

#### CROSS JOIN
```sql
-- CROSS JOIN : 두 테이블의 행을 결합한 데이터 조회
-- [MEMBER] 행 X [ORDER] 행
SELECT *
FROM [MEMBER] A
CROSS JOIN [ORDER] B
WHERE A.mem_no = '1000001'
-- mem_no가 [MEMBER] 및 [ORDER]에도 있으므로, A.mem_no 또는 B.mem_no로 명시해야함
```
- mem_no를 '1000001'로 필터했기 때문에 1행 X N행이 조회.
- mem_no가 [MEMBER] 및 [ORDER] 테이블에 모두 있기 때문에 A.mem_no로 정확한 테이블의 열 명시.

#### SELF JOIN
```sql
-- SELF JOIN : 한 테이블(MEMBER)을 두 테이블(A,B)로 하여 행을 결합한 데이터 조회
-- [MEMBER] 행 X [MEMBER] 행
SELECT *
FROM [MEMBER] A, [MEMBER] B
WHERE A.mem_no = '1000001'
-- mem_no가 [MEMBER]의 A,B에 있으므로, A.mem_no 또는 B.mem_no로 명시해야함
```
- mem_no를 '1000001'로 필터했기 때문에 1행 X N행이 조회.
- mem_no가 [MEMBER] A 및 [MEMBER] B 테이블에 모두 있기 때문에 A.mem_no로 정확한 테이블의 열 명시.

# 3. 서브 쿼리(Sub Query)

## 3-1 서브 쿼리란?
- 서브 쿼리(Sub Query)는 하나의 SQL 명령어에 포함된 SELECT 명령어.

![image](https://user-images.githubusercontent.com/75361137/154440177-ba76b225-0baa-490e-b793-3987eee27f85.png)
- 하나의 SQL 명령어는 메인 쿼리(Main Query)라 하며, 서브 쿼리(Sub Query)는 SELECT절, FROM절, WHERE절 뒤에 위치하여 사용.

## 3-2 위치에 따른 서브 쿼리
- 서브 쿼리(Sub Query)는 주로 SELECT절, FROM절, WHERE절 뒤에 위치하여 사용.

### 3-2-1 SELECT절
```sql
-- SELECT절에 사용되는 서브 쿼리
SELECT *
        ,(SELECT gender
            FROM [MEMBER] B
            WHERE A.mem_no = B.mem_no) AS gender
FROM [ORDER] A
```
- SELECT절에 사용되는 서브 쿼리는 하나의 열처럼 사용되고, 이를 스칼라(Scala) 서브 쿼리라고 정의. -> 스칼라는 한번에 하나의 값만 보유할 수 있는 원자량.
- JOIN의 대체 표현식이지만, 데이터의 양이 많으면 실행 속도가 느려 거의 사용 X.

### 3-2-2 FROM절
```sql
SELECT *
FROM (
    SELECT mem_no, SUM(sales_amt) AS tot_amt -- 집계함수에 대한 열 이름
    FROM [ORDER]
    GROUP BY mem_no
) A -- 서브 쿼리에 대한 테이블명
```
- FROM절에 사용되는 서브 쿼리는 테이블처럼 사용되며, 이를 인라인 뷰(Inline View)서브 쿼리라고 정의.
- 테이블처럼 사용되기 때문에 열 이름 및 테이블명을 명시.

```sql
-- FROM절에 사용되는 서브 쿼리
-- [MEMBER] 및 [ORDER] 테이블 공통 값(mem_no) 결합
-- 1:1 관계
SELECT *
FROM (
    SELECT mem_no, SUM(sales_amt) AS tot_amt -- 집계함수에 대한 열 이름
    FROM [ORDER]
    GROUP BY mem_no
) A -- 서브 쿼리에 대한 테이블명
LEFT JOIN [MEMBER] B
ON A.mem_no = B.mem_no
```
- FROM절에 사용된 서브쿼리(A)와 [MEMBER] 테이블 간의 공통 값(mem_no)이 매칭되는 데이터만 조회.

### 3-2-3 WHERE절
- WHERE절 뒤에 위치한 쿼리는 일반 서브 쿼리로 정의. 종류는 2가지(단일, 다중).

#### 단일 행 서브쿼리

```sql
-- WHERE절에 사용되는 서브 쿼리(단일 행 서브쿼리)
-- 단일 행 : 서브 쿼리 결과가 단일 행
SELECT *
FROM [ORDER]
WHERE mem_no = (SELECT mem_no FROM [MEMBER] WHERE mem_no = '1000005')

-- 단일 행 확인
SELECT mem_no FROM [MEMBER] WHERE mem_no = '1000005'
```
- [MEMBER] 테이블의 [mem_no]가 '1000005'로 필터되어 [ORDER] 테이블이 조회.
- WHERE절에 사용된 '='은 비교 연산자.
- 단일 행 서브쿼리는 결과가 단일 행이어야 함.

#### 다중 행 서브쿼리

```sql
-- WHERE절에 사용되는 서브 쿼리(다중 행 서브쿼리)
-- 다중 행 : 서브 쿼리 결과가 여러 행
SELECT *
FROM [ORDER]
WHERE mem_no = (SELECT mem_no FROM [MEMBER] WHERE gender = 'man')

-- 다중 행 확인
SELECT mem_no FROM [MEMBER] WHERE gender = 'man'
```
- [MEMBER] 테이블의 [gender] 열이 'man'인 [mem_no]로 필터되어 [ORDER] 테이블이 조회.
- WHERE절에 사용된 'IN'은 리스트(List)의 값 중 하나라도 일치하면 되는 특수 연산자.
- 다중 행 서브 쿼리는 결과가 여러 행이어야 함.

# 4. 정리

## 4-1 데이터 조회(SELECT)
```sql
USE EDU -- [EDU] 데이터베이스 사용.

-- (1) SELECT, FROM : [ORDER] 테이블의 모든 컬럼을 조회하여라.
SELECT *
FROM [ORDER]

-- (2) WHERE : [shop_code]는 30 이상으로만 필터링하여라.
SELECT *
FROM [ORDER]
WHERE shop_code >= 30

-- (3) GROUP BY(+SUM 함수) : [mem_no]별 [sales_amt] 합계를 구하여라. (* SUM(sales_amt)의 컬럼명은 tot_amt)
SELECT mem_no, SUM(sales_amt) AS tot_amt
FROM [ORDER]
WHERE shop_code >= 30
GROUP BY mem_no

-- (4) HAVING : [sales_amt] 합계가 100000 이상으로만 필터링하여라.
SELECT mem_no, SUM(sales_amt) AS tot_amt
FROM [ORDER]
WHERE shop_code >= 30
GROUP BY mem_no
HAVING SUM(sales_amt) >= 100000

-- (5) ORDER BY(+DESC) : [sales_amt] 합계가 높은 순으로 정렬하여라.
SELECT mem_no, SUM(sales_amt) AS tot_amt
FROM [ORDER]
WHERE shop_code >= 30
GROUP BY mem_no
HAVING SUM(sales_amt) >= 100000
ORDER BY SUM(sales_amt) DESC
```

## 4-2 데이터 조회(SELECT) + 결합(JOIN) + 서브 쿼리(Sub Query)

#### JOIN 활용
```sql
USE EDU -- [EDU] 데이터 베이스 사용.

-- 데이터 조회(SELECT) + 결합(JOIN)
-- (1) [ORDER] 테이블 기준으로 [MEMBER] 테이블을 LEFT JOIN 하여라.
SELECT *
FROM [ORDER] A
LEFT JOIN [MEMBER] B
ON A.mem_no = B.mem_no

-- (2) [gender]별 [sales_amt] 합계를 구하여라.(* SUM(sales_amt)의 컬럼명은 tot_amt)
SELECT B.gender, SUM(sales_amt) AS tot_amt
FROM [ORDER] A
LEFT JOIN [MEMBER] B
ON A.mem_no = B.mem_no
GROUP BY B.gender

-- (3) [gender], [addr]별 [sales_amt] 합계를 구하여라.(* SUM(sales_amt)의 컬럼명은 tot_amt)
SELECT B.gender, B.addr, SUM(sales_amt) AS tot_amt
FROM [ORDER] A
LEFT JOIN [MEMBER] B
ON A.mem_no = B.mem_no
GROUP BY B.gender, B.addr
```

#### JOIN 및 Sub Query 활용
```sql
USE EDU -- [EDU] 데이터 베이스 사용.

-- 데이터 조회(SELECT) + 결합(JOIN) + 서브 쿼리(Sub Query)
-- (1) [ORDER] 테이블의 [mem_no]별 [sales_amt] 합계를 구하여라.(* SUM(sales_amt)의 컬럼명은 tot_amt)
SELECT mem_no, SUM(sales_amt) AS tot_amt
FROM [ORDER]
GROUP BY mem_no

-- (2) (1)을 FROM절 서브 쿼리를 사용하여. [MEMBER] 테이블을 LEFT JOIN 하여라.
SELECT *
FROM (
    SELECT mem_no, SUM(sales_amt) AS tot_amt
    FROM [ORDER]
    GROUP BY mem_no
    ) A
LEFT JOIN [MEMBER] B
ON A.mem_no = B.mem_no

-- (3) [gender], [addr]별 [tot_amt] 합계를 구하여라.(* SUM(tot_amt))의 컬럼명은 합계)
SELECT B.gender, B.addr, SUM(tot_amt) AS 합계
FROM (
    SELECT mem_no, SUM(sales_amt) AS tot_amt
    FROM [ORDER]
    GROUP BY mem_no
) A
LEFT JOIN [MEMBER] B
ON A.mem_no = B.mem_no
GROUP BY B.gender, B.addr
```
