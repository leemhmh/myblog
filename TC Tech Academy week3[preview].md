# TC Tech Academy week3
Preview

---
RDBMS

1. 테이블들의 연관 관계
## SQL Unique key
  - 테이블 칼럼에 중복 값 허용 X
  - 두 레코드가 한 열에 동일한 값을 갖는 것을 방지

## 고유키의 특징
  - Prime 키와 달리 NULL값을 허용
  - 하나의 NULL값만 허용
  - 중복 값을 가질 수 없음
  - 다른 테이블에서 Foreign Key로 사용 가능
  - 테이블에는 둘 이상의 Unique 열이 있을 수 있음

## 고유키 만들기

    CREATE TABLE table_name(
     column1 datatype UNIQUE KEY,
     column2 datatype,
     .....
     .....
     columnN datatype
    );

- 예
  ```
  CREATE TABLE CUSTOMERS (
    ID INT NOT NULL UNIQUE KEY,
    NAME VARCHAR(20) NOT NULL,
    AGE INT NOT NULL,
    ADDRESS CHAR (25),
    SALARY DECIMAL (18, 2)
  );
  ```

##  Prime key
  - 각 레코드를 고유하게 식별하는 열 또는 열의 조합
  - 테이블에 하나만 있을 수 있으나 하나 이상의 필드에서 정의할 수 있음
  - 여러 필드에서 만들어지는 경우 복합 키라고 부름

## Prime Key 특징
  - 고유한 값만 포함
  - null일 수 없음
  - 하나의 테이블에는 하나만 존재
  - 길이는 900바이트 초과 불가

## 만들기
```
CREATE TABLE table_name(
   column1 datatype,
   column2 datatype,
   column3 datatype,
   .....
   columnN datatype,
   PRIMARY KEY(column_name)
);
```

## Foreign Key
  - 한 테이블의 열이 다른 테이블의 Prime 키와 일치하여 두 테이블을 함께 연결할 수 있도록 함
  - 두 테이블 간의 참조 무결성을 유지하므로 Prime 키가 포함된 테이블을 삭제할 수 없음
  - 데이터베이스에 있는 모든 테이블의 고유 필드를 참조할 수 있음
  - Prime 키가 있는 테이블 => 부모 테이블, Foreign 키가 있는 키 => 자식 테이블

## Foreign Key 특징
  - 테이블의 중복성을 줄이는 데 사용
  - 여러 테이블의 데이터를 정규화(또는 데이터베이스의 데이터 구성)하는 데 도움을 줌

## 만들기
```
CREATE TABLE table_name (
    column1 datatype,
    column2 datatype,
    ...
    CONSTRAINT fk_name 
	FOREIGN KEY (column_name) 
	REFERENCES referenced_table(referenced_column)
);
```


    
