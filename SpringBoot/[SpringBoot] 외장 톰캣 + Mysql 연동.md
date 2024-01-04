## 외장 톰캣에 Mysql 연동하기

### 해당 이슈 미진행
- DB 설정은 application.properties에서 진행

- DB 설정을 톰캣에 잡아버리면 WAS가 바뀔 때마다 프로그램의 DB설정을 새롭게 잡아야하니
properties에서 잡아주는 것이 맞다.

- 해당 이슈는 더 이상 진행하지 않고 중단
<br>
<hr>
<br>
- 일반적으로 SpringBoot에 Mysql을 연동하기 위해서는 아래와 같은 스크립트를 `aplication.properties`에 적용한다.

```
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/데이터베이스이름?serverTimezone=UTC&characterEncoding=UTF-8
spring.datasource.username=아이디
spring.datasource.password=비번
```
<br>
<br>

- 해당 글에서는 `application.properties`가 아닌 외장 톰캣의 `xml`파일을 이용하여 설정해본다.

- 기존에 `application.properties`로 설정했던 프로젝트에서 MySQL 설정 부분을 주석처리한다.
  - [참고 : SpringBoot + MySQL + Mybatis 셋팅 방법](https://github.com/yejun95/Today-I-Learned/blob/master/SpringBoot/SpringBoot%20%2B%20MySQL%20%2B%20Mybatis%20%EC%85%8B%ED%8C%85%20%EB%B0%A9%EB%B2%95.md)

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/9e6a3fd4-c7db-4c49-ba92-96299da35b0d)

- pom.xml에 설정했던 MySQL dependency도 주석 처리 진행

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/13ed89e4-aec2-4589-b9d1-c76945a73eeb)
<br>
<hr>
<br>

### ✔ MySQL-Connector 설치
- https://downloads.mysql.com/archives/c-j/ 사이트에서 `mysql.zip` 파일을 다운로드
  - `pom.xml`에서 설정했던 버전과 동일한 8.0.28로 다운
  - `.tar` 파일은 Linux에서 사용하는 파일 형식
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/4233b93b-1578-403a-ac55-e32e68319a23)
<br>
<br>

- 압축 해제 후 나온 jar 파일을 tomcat의 webapps/ROOT/WEB-INF/lib 폴더에 넣는다.
  - 톰캣 전역 설정인 경우 바로 lib에 넣어도 되지만 ROOT 프로그램에만 적용할 것이기 때문에
  ROOT/WEB-INF/lib에 적용한 것

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/a07754b4-2284-42d3-b49f-4abfabc8b46c)

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/d241cda0-9faf-4cc0-9382-80b2cb53ef56)
<br>
<br>

### ✔ tomcat 설정
- 빌드된 ROOT.war 프로그램을 사용한다.

- `server.xml`에서 설정하여도 되지만 Conf/Catalina/localhost 경로에 `ROOT.xml`을 만들어서 진행한다.

- 아래의 스크립트를 `ROOT.xml`에 삽입
```
<Context>
   ...
   <Resource name="jdbc/test"
        auth="Container"
        type="javax.sql.DataSource"
        username="아이디"
        password="비밀번호"
        driverClassName="com.mysql.jdbc.Driver"
        url="jdbc:mysql://localhost:3306/데이터베이스이름" 
        maxActive="15"
        maxIdle="3"/>
        
   ...
</Context>
```
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/4d084fec-fa7e-46b0-87b3-cd6db5fd5326)


- maxActive="15": 풀에서 동시에 활성화될 수 있는 최대 커넥션 수를 지정, 여기서는 최대 15개의 활성 커넥션을 허용

- maxIdle="3": 풀에서 유지할 수 있는 최대 유휴 커넥션 수를 지정, 여기서는 최대 3개의 유휴 커넥션을 허용
<br>
<hr>
<br>

### ✔ build
- build를 통해 새롭게 설정된 war 파일을 톰캣에 넣는다.

- build faild 발생

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/304fdd9f-804b-4ffa-ae40-864e3c7f05dd)

```
Description:

Failed to configure a DataSource: 'url' attribute is not specified and no embedded datasource could be configured.

Reason: Failed to determine a suitable driver class
```
<br>
<br>

- Mybatis 설정만 남기고 MySQL 설정은 주석 처리 한 상태로 buiild를 진행하려고 하니 에러가 발생한 것 

- 나의 의도는 스프링부트에 MyBatis 설정은 냅두고 MySQL 설정만 톰캣에서 진행하려고 했다.
<br>
<hr>
<br>
