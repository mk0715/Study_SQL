# 1. SQL 기본 구조

## 1-1 SQL 이란?
- SQL(Structured Query Language)는 데이터를 추출 / 수집 / 가공 / 검색 / 분석하기 위한 언어.
- SQL이 사용되는 관계형 데이터베이스 관리 시스템(RDBMS)은 MS-SQL, MySQL, Oracle, PostgreSQL 등이 존재.
- SQL의 기본 문법은 거의 다 유사하기 때문에, 여기에서는 MS-SQL을 사용하나 다른 SQL문법도 사용 가능.
- 기본 용어
1. RDBMS(Relational Database Management System) : 관계형 데이터베이스를 생성, 수정, 관리할 수 있는 시스템
2. 관계형 데이터베이스(Relational Database) : 데이터 간의 관계를 테이블 구조로 나타내는 공간
3. 테이블(Table) : 행(row)과 열(Column)으로 이루어진 2차원 표

## 1-2 SQL Server 및 SSMS 설치하기
- 설치 완료.

## 1-3 SSMS 살펴보기
- 살펴 보기.

# 2. SQL 기본 명령어

## 2-1 SQL 기본 명령어 분류
- SQL 기본 명령어는 4가지로 분류
1. DDL(Data Definition Language) : 테이블 생성(Create), 변경(Alter, Rename), 삭제(Truncate, Drop)
2. DML(Data Manipulation Language) : 데이터 삽입(Insert), 조회(Select), 수정(Update), 삭제(Delete)
3. DCL(Data Control Language) : 특정 사용자에게 데이터 접근 권한 부여(Grant), 제거(Revoke)
4. TCL(Transaction Control Language) : DML 명령어를 실행(Commit), 취소(Rollback), 임시 저장(Savepoint)


## 2-2 테이블을 정의하자(DDL)
- DDL(Data Manipulation Language)은 테이블을 생성 / 변경 / 삭제하기 위한 명령어.
- 테이블은 행(row)와 열(column)로 이루어진 2차원 표
- 각 열(column)마다 데이터 형식이 정해져 있으며, 1가지 데이터 형식만 저장.
- 데이터 형식은 문자 / 숫자 / 날짜 / 논리형으로 분류되고, 저장소 크기(byte)에 따라 종류가 나뉨.

1. 숫자형

| 데이터 형식 | 바이트 수 | 숫자 범위 | 설명 | 
| ---------------- | ---------------- | ---------------- | ---------------- |
| BIT | 1 | 0 or 1 or NULL | 논리형으로 참, 거짓으로 사용 |
| TINYINT | 1 | 0~255 | 양의 정수 |
| SMALLINT | 2 |-32768 ~ +32767 | 정수 |
| INT | 4 | 약 -21억 ~ + 21억 | 정수 |
| BIGINT | 8 | -2^63 ~ +2^63 | 정수 |
| DECIMAL(p, s) | 5 ~ 17 | -10^38 ~ +10^38-1 | 고정소수점(p 전체 자릿수, s 소수점 자릿수) |
| NUMERIC | 5 ~ 17 | -10^38 ~ +10^38 -1 | DECIMAL과 같은 데이터 형식이지만 근사값 |
| FLOAT(p) | 4 ~ 8 |  | p가 25미만이면 4바이트, 25이상이면 8바이트 할당 |
| REAL | 4 |  | FLOAT(24)와 같은 데이터 형식 |
| MONEY | 8 | -2^63 ~ +2^63 -1 | 화폐 단위로 사용 |
| SMALLMONEY | 4 | 약 -21억 ~ + 21억 | 화폐 단위로 사용 |

2. 문자형

| 데이터 형식 | 바이트 수 | 설명 | 
| ---------------- | ---------------- | ---------------- |
| CHAR(n) | 0 ~ 8000 | 고정길이 문자형 |
| NCHAR(n) | 0 ~ 8000 | 유니코드 고정길이 문자형 * 한글 0 ~ 4000자 |
| VARCHAR(n) | 0 ~ 2^31 -1 | 가변길이 문자형 * VARCHAR(MAX) 2GB |
| NVARCHAR(n) | 0 ~ 2^31 -1 | 유니코드 가변길이 문자형 * NVARCHAR(MAX) 2GB |
| BINARY(n) | 0 ~ 8000 | 고정길이 이진 데이터 값 |
| VARBINARY(n) | 0 ~ 2^31 -1 | 가변길이 이진 데이터 값 * VARBINARY(MAX) 2GB |

3. 날짜형

| 데이터 형식 | 바이트 수 | 설명 | 
| ---------------- | ---------------- | ---------------- |
| DATETIME | 8 | YYYY-MM-DD 시:분:초 * 정확도는 1/1000초 |
| DATETIME2 | 6 ~ 8 | YYYY-MM-DD 시:분:초 * 정확도는 100 나노초 |
| DATE | 3 | YYYY-MM-DD |
| TIME | 5 | 시:분:초 * 정확도는 100나노초 |

- 각 데이터 형식마다 저장소 크기(바이트 수)를 지정하는 이유는 데이터 저장 공간 낭비를 줄이기 위함.


### 2-2-1 테이블 생성(Create)

```sql
CREATE TABLE [회원테이블] (
    [회원번호] VARCHAR(20) PRIMARY KEY,
    [이름] VARCHAR(20),
    [성별] VARCHAR(2),
    [나이] INT,
    [가입금액] MONEY,
    [가입일자] DATE NOT NULL,
    [수신동의] BIT
)
```
- PRIMARY KEY와 NOT NULL은 데이터 제약 조건 -> 후에 다룰 예정.
- 주석(comment)를 남길 때에는 단일 라인 주석(--), 블록 주석(/* */) 사용.

### 2-2-2 테이블 열 추가(Alter)

```sql
ALTER TABLE [회원테이블] ADD [몸무게] REAL
```
- 열 추가할 때 반드시 데이터 형식 지정.

### 2-2-3 테이블 열 형식 변경(Alter)
```sql
ALTER TABLE [회원테이블] ALTER COLUMN [몸무게] INT
```
- 변경하고자 하는 테이블 열을 선택하고 형식 선택.

### 2-2-4 테이블 열 이름 변경(Rename)
```sql
SP_RENAME '[회원테이블].[몸무게]', '몸무게(kg)'
```
- 변경 하고자하는 테이블과 열을 선택하고 바꾸는 이름 설정.

### 2-2-5 테이블명 변경(Rename)
```sql
SP_RENAME '[회원테이블]', 'MEMBER'
``` 
- 변경하고자하는 테이블을 선택하고 바꾸는 이름 설정.

### 2-2-6 데이터 삭제(Truncate) 및 테이블 삭제(Drop)
```sql
TRUNCATE TABLE [MEMBER]
DROP TABLE [MEMBER]
```
- TRUNCATE로 지정하면 테이블의 모든 행 데이터를 삭제.
- DROP은 테이블을 완전히 삭제.


## 2-3 데이터를 조작하자(DML)
- DML(Data Manipulation Language)은 데이터를 삽입 / 조회 / 수정 / 삭제하기 위한 명령어.

### 2-3-1 데이터 삽입(Insert)
```sql
INSERT INTO [회원테이블] VALUES ('A10001', '모원서', '남', 33, 100000, '2020-01-01', 1);
INSERT INTO [회원테이블] VALUES ('A10002', '김영화', '여', 29, 200000, '2020-01-02', 0);
INSERT INTO [회원테이블] VALUES ('A10003', '홍길동', '남', 29, 300000, '2020-01-03', NULL);
```
- 데이터 형식이 문자 / 날짜형이면 ''(작은 따옴표) 필요.
- 열 구분은 쉼표(,)가 필요.
- PRIMARY KEY로 지정되있는 열에는 중복 키가 존재할 수 없음. (여기서는 회원번호)
- NOT NULL로 지정되어있는 열에는 NULL 값이 존재할 수 없음. (여기서는 가입일자)

### 2-3-2 데이터 조회(Select)
```sql
SELECT *
FROM [회원테이블]

SELECT [회원번호],
       [이름] AS [성명],
       [가입일자],
       [나이]
FROM [회원테이블] 
```
- *은 테이블에 있는 모든 열을 조회
- AS(Aliases)는 임시로 별칭을 생성. (생략 가능)

### 2-3-3 데이터 수정(Update)
```sql
UPDATE [회원테이블]
SET [나이] = 30

UPDATE [회원테이블]
SET [나이] = 34
WHERE [회원번호] = 'A10001'
```
- SET은 변경할 열 및 값을 설정.
- WHERE은 테이블을 특정 조건으로 필터하는 명령어.

### 2-3-4 데이터 삭제(Delete)
```sql
DELETE
FROM [회원테이블]

DELETE
FROM [회원테이블]
WHERE [회원번호] = 'A10001'
```

## *Delete vs Truncate vs Drop*
![image](https://user-images.githubusercontent.com/75361137/154215400-94f967f3-bc61-4a90-abfc-eb88c3398f46.png)
- Delete는 데이터만 삭제.
- Truncate는 데이터 + 테이블 공간만 삭제.
- Drop은 테이블 전체가 삭제.


## 2-4 데이터 접근을 제어하자(DCL)
- DCL(Data Control Language)은 데이터베이스 관리자(DBA)가 특정 사용자(User)들에게 데이터 접근 권한을 부여하거나 이들의 접근 권한을 제거하기 위한 명령어.
![image](https://user-images.githubusercontent.com/75361137/154216329-3d226143-7e5f-4a5f-9818-73a52bd18ac5.png)

### 2-4-1 권한 부여(Grant)
```sql
GRANT SELECT, INSERT, UPDATE, DELETE ON [회원테이블] TO MWS WITH GRANT OPTION
```
- WITH GRANT OPTION은 특정 사용자(MWS)가 받은 권한을 다른 사용자에게 부여할 수 있는 명령어.

### 2-4-2 권한 제거(Revoke)
```sql
REVOKE SELECT, INSERT, UPDATE, DELETE ON [회원테이블] TO MWS CASCADE
```
- CASCADE는 특정 사용자(MWS)가 다른 사용자에게 부여한 권한도 연쇄적으로 취소할 수 있는 명령어.


## 2-5 명령어를 제어하자(TCL)
- TCL(Transaction Control Language)은 데이터 조회 / 삽입 / 수정 / 삭제를 제어하기 위한 명령어.
- 트랜잭션(Transaction)은 데이터 조회(Select), 삽입(Insert), 수정(Update), 삭제(Delete)의 DML 명령문들을 하나의 그룹으로 처리하는 논리적인 작업 단위.
- EX) 하나의 그룹으로 처리하는 일 예시 

(1) A의 계좌에서 100만원 예금 인출(UPDATE)

(2) B의 계좌로 100만원 예금 입금(UPDATE)

(3) A 및 B 계좌 조회(SELECT)

- (1) ~ (3) 중에 하나라도 에러가 발생하면 트랜잭션(Transaction) 작업 전 상태로 취소 -> ROLLBACK.
- 에러가 발생하지 않는다면 작업 실행 -> COMMIT.
- 즉, 트랜잭션(Transaction)은 여러 DML 명령어를 하나의 그룹으로 처리하여 하나의 명령어라도 에러가 발생하면, 트랜잭션(Transaction)에 포함된 모든 DML 명령어를 취소.

### 2-5-1 실행(Commit)
```sql
USE EDU

BEGIN TRAN

DELETE FROM [회원테이블]

COMMIT
```
- BEGIN TRAN은 TCL 명령어를 시작하기 위한 선언 명령어.
- COMMIT이 실행되면 되돌릴 수 없음.

### 2-5-2 취소(Rollback)
```sql
USE EDU

BEGIN TRAN

SELECT * FROM [회원테이블]

DELETE FROM [회원테이블]

SELECT * FROM [회원테이블]

ROLLBACK

SELECT * FROM [회원테이블]
```
- ROLLBACK을 함으로써 DELETE 했던 명령어들이 취소됨.

### 2-5-3 임시 저장(Savepoint)
```sql
USE EDU /* 1. [EDU] 데이터 베이스를 사용. */

BEGIN TRAN /* 2. TCL 시작 */

/* 3. SAVEPOINT.1 : [회원테이블]에 'A10001' 회원 데이터 삽입(INSERT) */
SAVE TRAN S1;
INSERT INTO [회원테이블] VALUES ('A10001', '모원서', '남', 33, 100000, '2020-01-01', 1);

/* 4. SAVEPOINT.2 : 'A10001' 나이 34로 수정(UPDATE) */
SAVE TRAN S2;
UPDATE [회원테이블] SET [나이] = 34 WHERE [회원번호] = 'A10001'

/* 5. SAVEPOINT.3 : [회원테이블]에 'A10003' 회원 데이터 삭제(DELETE) */
SAVE TRAN S3;
DELETE FROM [회원테이블] WHERE [회원번호] = 'A10003'

SELECT * FROM [회원테이블]

/* 6. S3 지정 취소 */
ROLLBACK TRAN S3;
SELECT * FROM [회원테이블]

/* 7. S2 지정 취소 */
ROLLBACK TRAN S2;
SELECT * FROM [회원테이블]

/* 8. S1 지정 취소 */
ROLLBACK TRAN S1;
SELECT * FROM [회원테이블]
```

# 3. 정리
- SQL 명령어의 특징으로는
- 대/소문자 구분 없음.
- 띄어쓰기 및 들여쓰기 간격 제한이 없음.
- 날짜 및 문자형에는 작은따옴표('')가 필요하며, 숫자형은 불필요.
