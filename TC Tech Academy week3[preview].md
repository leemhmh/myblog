# TC Tech Academy week3
Preview

---
RDBMS

# 1. 테이블들의 연관 관계
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

# 2. Indexing
## 인덱스란
- SQL 인덱스는 데이터 검색 프로세스의 속도를 높이는 데 사용되는 특수 조회 테이블
- 데이터베이스 내에 자체 스토리지 공간이 필요

## 만들기
```
CREATE INDEX index_name ON table_name;
```

## 인덱스 유형
- Unique Index
  - 고유 인덱스는 테이블에 중복 값 삽입 허용 X
```
CREATE UNIQUE INDEX index_name
on table_name (column_name);
```
- Single-Column Index
  - 단일 열 인덱스는 하나의 테이블 열에만 만들어짐
```
CREATE INDEX index_name
ON table_name (column_name);
```
- Composite Index
  - 복합 인덱스는 테이블의 두 개 이상의 열에 만들 수 있는 인덱스
```
CREATE INDEX index_name
on table_name (column1, column2);
```
- Implicit Index
  - 오브젝트가 작성될 때 데이터베이스 서버에 의해 자동으로 작성되는 인덱스

 ## 인덱스 사용을 피해야 하는 상황
 - 작은 테이블일 경우
 - 대량의 일괄 처리 업데이트 또는 삽입 작업이 자주 발생하는 테이블일 경우
 - 많은 수의 NULL 값이 포함된 열
 - 자주 조작되는 열

# 3. Transaction

## 트랜잭션이란
- 데이터베이스에 대해 수행되는 작업 단위
- 사용자 수동 수행이든 데이터베이스 프로그램의 자동 수행이든 논리적 순서로 수행되는 작업 단위 또는 시퀀스
- 하나 이상의 변경 내용을 데이터베이스에 전파하는 것

## 트랜잭션의 속성

ACID(Atomicity, Consistency, Isolation, Durability)

- 원자성 : 작업 단위 내의 모든 작업이 성공적으로 완료되도록 함, 그렇지 않으면 트랜잭션이 실패 지점에서 중단되고 모든 이전 작업이 이전 상태로 롤백됨
- 일관성 : 데이터베이스가 제대로 성공적으로 커밋 된 트랜잭션에 상태를 변경하는지 확인
- 격리 : 트랜잭션이 독립적으로 작동하고 서로 투명하게 작동
- 내구성 : 결과 또는 커밋 된 트랜잭션의 효과가 시스템 오류의 경우에도 지속되도록 함

### 트랜잭션 제어
- Commit: 변경 사항을 저장
- Rollback: 변경 사항을 롤백
- Savepoint: 롤백 할 트랜잭션 그룹 내에서 포인트를 생성
- Set Transaction : 트랜잭션에 이름을 배치

- 트랜잭션 제어 명령은 INSERT, UPDATE 및 DELETE와 같은 DML 명령에서만 사용됨. 
- 이러한 작업은 데이터베이스에서 자동으로 커밋되므로 테이블을 만들거나 삭제하는 동안에는 사용할 수 없습니다.

## Spring Data Jpa 활용
- ORM(Object-Relational Mapping)은 Java 오브젝트를 데이터베이스 테이블로 변환하는 프로세스
- 이를 통해 SQL 없이 관계형 데이터베이스와 상호 작용
- JPA(Java Persistence API)는 Java 애플리케이션에서 데이터를 지속하는 방법을 정의하는 스펙
- JPA의 주요 초점은 ORM 계층

---

- JPA는 자바에서 관계형 데이터베이스를 사용하는 방식을 정의한 인터페이스
- 인터페이스이므로 실제 사용을 위해서는 ORM 프레임워크를 추가로 선택해야 함. 대표적으로 Hibernate를 사용
- 하이버네이트의 목표는 자바 객체를 통해 데이터베이스 종류에 상관없이 데이터베이스를 자유자재로 사용할 수 있게 하는 데에 있음. 내부적으로는 JDBC API를 사용

---

- 엔티티 : 데이터베이스의 테이블과 매핑되는 객체, 본질적으로는 자바 객체와 크게 다르지 않으나, 테이블과 직접 연결된다는 특징이 있어 구분됨. 데이터베이스에 영향을 미치는 쿼리를 실행하는 객체
- 엔티티 매니저 : 엔티티를 관리해 데이터베이스와 애플리케이션 사이에서 객체를 생성, 수정, 삭제하는 등의 역할을 함. 이를 만드는 곳이 엔티티 매니저 팩토리, 요청이 들어오면 매니저를 생성하여 정보 저장

---

- 스프링 부트는 내부에서 엔티티 매니저 팩토리를 하나만 생성해서 관리, @PersistenceContext 또는 @Autowired 어노테이션을 사용하여 엔티티 매니저를 사용
- 영속성 컨텍스트란 : 엔티티를 관리하는 가상의 공간. 1차 캐시, 쓰기 지연, 변경 감지, 지연 로딩이라는 특징이 있음
- 1차 캐시 : 캐시의 키는 @Id 어노테이션이 달린 기본(Prime) 키 역할을 하는 식별자이며 값은 엔티티. 엔티티를 조회하면 1차 캐시에서 데이터를 조회하고 값이 있으면 반환, 없으면 데이터베이스에서 조회해 1차 캐시에 저장 후 반환
- 쓰기 지연 : 트랜잭션을 커밋하기 전까지는 데이터베이스에 실제로 쿼리를 보내지 않고 모았다가 트랜잭션을 커밋하면 모았던 쿼리를 한 번에 실행. DB시스템의 부담을 줄일 수 있음 
- 변경 감지 : 트랜잭션을 커밋하면 1차 캐시에 저장되어 있는 엔티티의 값과 현재 엔티티의 값을 비교해서 변경된 값이 있다면 변경 사항을 감지해 변경된 값을 DB에 자동으로 반영. DB 시스템 부담 감소
- 지연 로딩 : 쿼리로 요청한 데이터를 애플리케이션에 바로 로딩하는 것이 아니라 필요할 때 쿼리를 날려 데이터를 조회하는 것을 의미(반대 의미의 즉시 로딩도 있음)

