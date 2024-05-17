 ## Conneciton Pool(DBCP)이란?

 - 웹 컨테이너(WAS)가 실행되면서 DB와 미리 connection을 해놓은 객체들을 pool에 저장해두었다가<br>
 클라이언트 요청이 오면 connection을 빌려주고, 처리가 끝나면 다시 connection을 반납 받아<br>
 pool에 저장하는 방식
<br>

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/646f5b5e-c6b9-47b5-b06f-026e2df1295f)
<br>

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/5a296fde-5806-4e8c-bd47-3236a78916b5)
<br>

1. 서버(WAS)는 미리 DB와 일정 수의 connection을 맺은 후 connection 객체를 Pool에 저장합니다.

2. 사용자의 요청이 발생하게 되면 서버(WAS)는 Pool에 connection을 요청합니다.

3. connection을 얻은 후 쿼리를 실행하여 데이터를 read / write 합니다.

4. connection을 Pool에 반납합니다.
<br>
<hr>
<br>

### ✔ 사용 이유

- 매번 DB에 직접 연결해서 처리하는 경우, 드라이버 로드 후 커넥션 객체를 받아와야 한다.<br>
그러면 클라이언트 요청시 마다 연결/종료가 반복되기 때문에 비효율적.

- 이러한 문제를 해결하기 위해 pool에서 빌림/반납 반복
<br>

JDBC 예제 소스 : 매번 연결
```java
try {
    sql = "SELECT * FROM T_BOARD"

    // 1. 드라이버 연결 DB 커넥션 객체를 얻음
    connection = DriverManager.getConnection(DBURL, DBUSER, DBPASSWORD);

    // 2. 쿼리 수행을 위한 PreparedStatement 객체 생성
    pstmt = conn.createStatement();

    // 3. executeQuery: 쿼리 실행 후
    // ResultSet: DB 레코드 ResultSet에 객체에 담김
    rs = pstmt.executeQuery(sql);
    } catch (Exception e) {
    } finally {
        conn.close();
        pstmt.close();
        rs.close();
    }
}
```
<br>

### ✔ 특징
- WAS가 실행되면서 connection 객체를 미리 pool에 생성
 
- HTTP요청에 따라 pool에서 connection 객체를 가져다 쓰고 반환
 
- 위와 같은 방식으로 물리적 DB 연결 부하 줄임
 
- pool에 미리 connection이 생성되어 있기 때문에 연결 시간이 빠르다.
 
- 커넥션 재사용 및 커넥션 수를 제한적으로 설정 가능
<br>
<hr>
<br>

### ✔ 문제점
**동시 접속자가 많을 경우**
<br>

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/21701ced-f414-4077-b927-d54397d523bb)
<br>

- connection이 없을 경우 클라이언트는 connection이 반환될 때까지 번호순대로 대기 상태가 된다.
  - 3초 이내로 응답이 없을 시, 에러 페이지로 신속하게 이동시키는 것도 하나의 방법
 <br>
 
- connection pool 작게 설정
  - 대기시간이 늘어나지만 적은 메모리 사용
  - 원활한 서비스가 불가능
<br>

- connection pool 크게설정
  - 대기시간이 줄어드는 대신 메모리 소모가 크다.
  - 서버 성능 이상의 pool을 설정하면 메모리를 많이 사용하여 오히여 성능 저하 발생
<br>
<hr>
<br>

### ✔ Summary
- connection Pool을 사용한다면 유저 수에 따라 connection 수를 적절하게 지정해야 한다.
 
- DB Pool 설정은 성능에 직접적으로 영향을 주는 중요한 요인이므로 최적화된 설정 검증을 위해서는 반드시 성능 테스트를 수행해야 한다.
  - initialSize	: BasicDataSource Class 생성 후 최초로 getConnection() 메서드를 호출할 때 Connection Pool에 채워 넣을 Connection의 개수
  - maxActive	: 동시에 사용할 수 있는 최대 Connection 개수(기본값:8). maxTotal(2.x)
  - minIdle	: 최소한으로 유지할 Connection의 개수(기본값:0)
  - maxIdle	: Connection Pool에 반납할 때 최대로 유지될 수 있는 Connection의 개수(기본값:8
<br>

- 
<br>
<hr>
<br>

**Reference**<br>

[Jayon : Connection Pool이란?](https://steady-coding.tistory.com/564)<br>
[linked2ev : 커넥션 풀(Connection pool)이란?](https://linked2ev.github.io/spring/2019/08/14/Spring-3-%EC%BB%A4%EB%84%A5%EC%85%98-%ED%92%80%EC%9D%B4%EB%9E%80/)<br>
[gwon713 : Nodejs DB connection pool](https://velog.io/@gwon713/Nodejs-MySQL-DB-connection-pool)
