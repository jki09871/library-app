# 도서 관리 애플리케이션

---

## 이 애플리케이션은 Spring Boot, JPA, H2/MySQL을 사용하여 개발된 간단한 도서 관리 시스템이다. 이 시스템은 사용자와 도서를 관리하고, 대출 및 반납 기록을 추적하며, 도서 및 사용자에 대한 기본적인 CRUD 작업을 처리할 수 있다.

### 주요 기능
#### 1. 도서 관리
도서 생성: 새로운 도서를 추가할 수 있다.\
도서 대출: 사용자가 도서를 대출할 수 있다.\
도서 반납: 대출한 도서를 반납할 수 있다.
#### 2. 사용자 관리
사용자 생성: 새로운 사용자를 추가할 수 있다.\
사용자 조회: 등록된 사용자 목록을 확인할 수 있다.\
사용자 수정: 사용자 정보를 업데이트할 수 있다.\
사용자 삭제: 사용자를 삭제할 수 있다.
#### 3. 대출 관리
대출 기록 관리: 사용자가 대출한 도서의 대출 및 반납 여부를 추적할 수 있다.\
대출 상태 확인: 이미 대출된 도서는 중복 대출을 방지할 수 있다.\
### 기술 스택
Spring Boot: 애플리케이션 프레임워크\
JPA (Java Persistence API): 데이터베이스 ORM (객체 관계 매핑)\
H2 Database: 개발 환경에서 사용되는 인메모리 데이터베이스\
MySQL: 운영 환경에서 사용되는 데이터베이스\
Spring Data JPA: JPA와 함께 사용되는 데이터 접근 계층
### 설정
#### 로컬 환경
로컬 환경에서는 H2 데이터베이스를 사용하며, 애플리케이션 실행 시 메모리에서 데이터베이스가 생성되고 애플리케이션 종료 시 데이터베이스가 사라진다.
```
spring:
  datasource:
    url: "jdbc:h2:mem:library;MODE=MYSQL;NON_KEYWORDS=USER"
    username: "sa"
    password: ""
    driver-class-name: org.h2.Driver
  jpa:
    hibernate:
      ddl-auto: create
    properties:
      hibernate:
        show_sql: true
        format_sql: true
        dialect: org.hibernate.dialect.H2Dialect
  h2:
    console:
      enabled: true
      path: /h2-console

```
#### 운영 환경
운영 환경에서는 MySQL 데이터베이스를 사용하며, 데이터베이스 설정은 다음과 같다.
```
spring:
  datasource:
    url: "jdbc:mysql://localhost/library"
    username: ""
    password: ""
    driver-class-name: com.mysql.cj.jdbc.Driver
  jpa:
    hibernate:
      ddl-auto: none
    properties:
      hibernate:
        show_sql: true
        format_sql: true
        dialect: org.hibernate.dialect.MySQL8Dialect
```
#### 실행 방법
#### 로컬 개발 환경에서 실행
애플리케이션을 실행한 후, 브라우저에서 http://localhost:8080/h2-console로 접속하여 H2 데이터베이스 콘솔에 접속할 수 있다. (JDBC URL: jdbc:h2:mem:library)

#### 운영 환경에서 실행
MySQL 데이터베이스 설정을 맞춘 후, 애플리케이션을 실행하여 운영 환경에서 동작하게 할 수 있다.

#### API 엔드포인트
#### 1. 도서 관련 API
POST /book: 도서를 추가한다.\
POST /book/loan: 도서를 대출한다.\
PUT /book/return: 대출된 도서를 반납한다.
#### 2. 사용자 관련 API
POST /user: 사용자를 추가한다.\
GET /user: 사용자 목록을 조회한다.\
PUT /user: 사용자 정보를 수정한다.\
DELETE /user: 사용자를 삭제한다.
#### 데이터베이스 스키마
User: 사용자 정보 (이름, 나이)\
Book: 도서 정보 (이름)\
UserLoanHistory: 사용자 대출 기록 (사용자, 도서명, 반납 여부)


# 배포 방법
## AWS EC2사용
