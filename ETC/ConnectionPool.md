 ## Conneciton Pool(DBCP)이란?

 - 웹 컨테이너(WAS)가 실행되면서 DB와 미리 connection을 해놓은 객체들을 pool에 저장해두었다가<br>
 클라이언트 요청이 오면 connection을 빌려주고, 처리가 끝나면 다시 connection을 반납 받아<br>
 pool에 저장하는 방식
<br>

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/646f5b5e-c6b9-47b5-b06f-026e2df1295f)
<br>


<br>
<hr>
<br>

**✔ 사용 이유**

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

**✔ 특징**

- WAS가 실행되면서 connection 객체를 미리 pool에 생성
- HTTP요청에 따라 pool에서 connection 객체를 가져다 쓰고 반환
- 위와 같은 방식으로 물리적 DB 연결 부하 줄임
- pool에 미리 connection이 생성되어 있기 때문에 연결 시간이 빠르다.
- 커넥션 재사용 및 커넥션 수를 제한적으로 설정 가능
<br>

**동시 접속자가 많을 경우**

- connection이 없을 경우 클라이언트는 connection이 반환될 때까지 번호순대로 대기상태
- WAS에서 커넥션 풀 크게 설정 => 메모리 소비가 크다.
<br>
<hr>
<br>

**Reference**<br>

[Jayon : Connection Pool이란?](https://steady-coding.tistory.com/564)<br>
[linked2ev : 커넥션 풀(Connection pool)이란?](https://linked2ev.github.io/spring/2019/08/14/Spring-3-%EC%BB%A4%EB%84%A5%EC%85%98-%ED%92%80%EC%9D%B4%EB%9E%80/)
