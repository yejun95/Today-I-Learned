## SpringBoot + MySql + Mybatis 연동

### ✔ dependency
- maven dependency에 mysql을 추가한다.

- 자신의 mysql과 맞는 버전 선택

```
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.28</version>
</dependency>
```
<br>

- mybatis dependency 추가

```
 <dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>1.3.2</version>
</dependency>
```

<br>
<hr>
<br>

### ✔ application.properties 설정
- 아래의 스크립트를 `application.properties`에 삽입한다.

```
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/my_db?serverTimezone=UTC&characterEncoding=UTF-8
spring.datasource.username=아이디
spring.datasource.password=비밀번호


# MyBatis 매퍼 XML 파일의 위치 설정
mybatis.mapper-locations=classpath:mapper/**/*.xml

# MyBatis 구성 설정
mybatis.configuration.map-underscore-to-camel-case=true
```

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/06431f63-2853-46cc-bdd3-f56bb8298f47)
<br>
<br>

- classpath:mapper/**/*.xml
  - 클래스패스 상에 있는 mapper 디렉토리와 그 하위 디렉토리에 위치한 모든 XML 파일을MyBatis 매퍼로 간주한다.

- mybatis.configuration.map-underscore-to-camel-case=true 
  - 데이터베이스 컬럼 이름에서 언더스코어(_)를 제거하고 카멜 케이스로 변환할 지 여부를 지정한다.
  - 스네이크 케이스 -> 카멜 케이스 변경
<br>
<hr>
<br>

### ✔ 패키지 및 폴더 생성
**entity 설정**
- mysql 컬럼과 데이터를 주고 받을 entity 생성

- `com.example.demo.entity` / User.java 생성

- 컬럼명 입력 후 getter, setter 설정

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/eb18e67f-920c-4474-aba3-886c2c1b74e5)
<br>
<br>

**mapper 설정**
- 패키지 및 인터페이스 생성
  - `com.example.demo.mapper` / UserMapper.java
  -  전체 유저리스트 조회 위한 함수 생성

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/24bfa7c2-9b32-4785-b88c-e55925026672)
<br>
<br>

- sql 문을 입력할 `mapper.xml` 생성
  - `src/main/resources/mapper/Mapper.xml

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/c9f6326f-ed6a-4f56-b3f3-d30e787fbdfe)
<br>

- namespace : 동기화될 mapper 인터페이스가 있는 패키지

- id : mapper 인터페이스에서 설정한 함수명

- resultType : 리턴될 데이터의 entity 설정
<br>
<hr>
<br>

### ✔ controller 설정
**Autowird**
- 자바 객체(Bean)을 찾아서 의존성을 주입한다. (Dependency Injection)

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/9e2e33bd-b326-4845-84e9-cc47ae372971)
<br>
<br>

- 이후 생성했던 mapper 함수를 불러와 데이터를 리턴 받는다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/f4b197e0-c0ab-4a6b-978c-7f181d3afad6)
<br>
<hr>
<br>

### ✔ Mysql 데이터 삽입
- 데이터가 리턴이 잘 되는지 확인하기 위하여 MySql에 가데이터를 넣는다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/6837e30a-d342-4020-8f01-2381af72e68f)
<br>
<br>

- 스프링부트 실행 후 브라우저 요청 확인
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/897b73ba-149d-4026-978d-a73647b02a67)


