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

### API 엔드포인트
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

---

# 배포 방법

1. 준비물
   AWS EC2 인스턴스 (Amazon Linux 2 AMI)
   키 페어 파일 (.pem)
   Spring Boot 애플리케이션 소스코드 (GitHub 저장소)
   필요한 프로그램: Git, Java 11, MySQL 8.0
2. EC2에 접속하기
   EC2 IP 주소 확인: AWS 관리 콘솔에서 EC2 인스턴스의 퍼블릭 IP 확인.

SSH 접속:

```
ssh -i <키페어 경로>/<키페어 이름>.pem ec2-user@<EC2 퍼블릭 IP>
```
접근 권한 오류 해결: 키 페어 권한 변경이 필요한 경우:

```
chmod 400 <키페어 경로>/<키페어 이름>.pem
```
3. 프로그램 설치
   시스템 업데이트:
```
sudo yum update -y
```

Git 설치:
```
sudo yum install git -y
```
Java 11 설치:
```
sudo yum install java-11-amazon-corretto -y
java -version
```

MySQL 설치:
https://www.inflearn.com/community/questions/1295620/gpg-%EC%B2%B4%ED%81%AC-%EC%98%A4%EB%A5%98

```
sudo rpm -ivh mysql80-community-release-el7-5.noarch.rpm
sudo yum install mysql-community-server -y
sudo systemctl restart mysqld
```
MySQL 초기 비밀번호 확인:
```
sudo cat /var/log/mysqld.log | grep "A temporary password"
```
MySQL 접속:

```
mysql -u root -p
```
임시 비밀번호를 입력 후 새 비밀번호 설정:

```
ALTER user 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '새_비밀번호';
```
4. 애플리케이션 배포
   GitHub에서 프로젝트 클론:

```
git clone <GitHub 저장소 URL>
cd <클론된 폴더>
```
Gradle 빌드:
빌드 전 메모리 스왑 설정 (EC2 메모리 부족 방지):
```
sudo dd if=/dev/zero of=/swapfile bs=128M count=16
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
```
빌드 실행:
```
chmod +x ./gradlew
./gradlew build -x test
```

서버 실행:
포그라운드 실행 (테스트용):
```
java -jar build/libs/<빌드된 JAR 파일명>.jar --spring.profiles.active=dev
```
백그라운드 실행:
```
nohup java -jar build/libs/<빌드된 JAR 파일명>.jar --spring.profiles.active=dev &
```
애플리케이션 확인:
브라우저에서 다음 URL 접속:
```
http://<EC2 퍼블릭 IP>:8080/v1/index.html
```


참고 : 인프런 강의(자바와 스프링 부트로 생애 최초 서버 만들기, 누구나 쉽게 개발부터 배포까지! [서버 개발 올인원 패키지]) 
https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-%EC%84%9C%EB%B2%84%EA%B0%9C%EB%B0%9C-%EC%98%AC%EC%9D%B8%EC%9B%90/dashboard
