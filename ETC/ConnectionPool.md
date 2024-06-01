![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/fd59cdc2-8523-41d2-b011-ddc441f1dc90) ## Conneciton Pool(DBCP)이란?
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

## 내용 추가 2024-05-23

### ✔ connection이 비용이 큰 이유
- connection pool을 사용하는 이유는 연결 시간을 줄여서 자원의 소모를 최소화하기 위함이다.

- 그렇다면 이러한 connection이 왜 비용이 많이 드는 것인가?

- Database는 최초 접속 시 TCP 통신을 하기 때문이다.

- 아래 그림을 봐보자

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/c36bd952-db38-4a26-af5d-707ecfe60db3)
> TCP의 3-way-handshake 과정
<br>

- 연결이라는 것은 클라이언트와 서버가 TCP 연결을 하는 것을 말한다.

- 이때 synchronize와 acknowledgment를 서로 주고 받으면서 연결이 이루어지므로, 해당 과정에서 높은 비용이 발생하는 것이다.

- 또한 데이터 전송이 종료되면 사용한 리소스를 반환하기 위해 4-way-handshake 과정도 수행한다.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/f6d9babd-6405-420e-9138-601bdc31ccc4)
> TCP의 4-way-handshake 과정
<br>

- 즉, connection pool을 사용하지 않으면 이러한 TCP 연결과 해제를 반복하기 때문에 자원의 소모가 커<br>
이를 보완하기 위해 Connection Pool이란 개념이 나왔다.
<br>

➡ **wireshark를 이용한 handshake 과정 확인**
- 실제로 3-way-handshake 과정을 패킷 분석 툴을 사용하면 확인이 가능하다.

- 해당 글에서는 wireshark를 사용

- 먼저 VSCode에서 이용할 스크립트는 다음과 같다.

```javascript
'use strict';
const { PerformanceObserver, performance } = require('perf_hooks');
var util = require('util');
const mysql = require('mysql2');

var conn = mysql.createConnection({ 
    host : '127.0.0.1',  
    user : 'root',
    password : 'root',
    database : 'connectionpool'
  });

const pool = mysql.createPool({  // 커넥션 풀 생성
  host: 'localhost',
  user: 'root',
  password: 'root',
  database: 'connectionpool',
  connectionLimit: 5
})

conTest(true, 1);
conTest(false, 1);
conTest(false, 2);
conTest(false, 3);

async function conTest(boolean, num) {
  var t0 = performance.now();
  if(boolean) {
    conn.connect(); // mysql과 연결
    const sql = "insert into testdb values (null, 'test')"
    conn.query(sql, function(err, rows, fields)
    {
      if (err) {
        console.error('error connecting: ' + err.stack);
      }
    });
    var t1 = performance.now();
    conn.end(); // 연결 해제
  } else {
    pool.getConnection(function(err, connection) {
      if(err) 
          throw err;
        else {
          // query 실행 
          connection.query(
            "insert into testdb values (null, 'test')", function(err, results) {
          }); 
          // connection을 pool로 반환
          connection.release();
        }
    });  
    var t1 = performance.now();
  }

  if(boolean) {
    console.log(`getConnection ${num}차 연결 성능`, t1 - t0)
  } else {
    console.log(`Pool ${num}차 연결 성능`, t1 - t0)
  }
}
```
<br>

- 함수 `conTest()`는 if 조건을 통해 일반 connection과 pool 이용한 커넥션으로 나뉜다.

- 이를 각각 2번, 3번 실행시켜서 hand-shake 과정이 어떻게 일어나는지 확인해보자.
  - pool을 사용하지않는 함수는 mysql connect 비동기 문제로 인해 동일한 함수(`conTest(true, 1)`)를 두번 실행으로 진행
<br>
<br>

- 먼저 connection 연결로 `conTest(true, 1)` 함수를 2번 실행 (pool 실행 함수 4개는 주석 처리)

```
$ node nodeQueryConnectionPool.js 
getConnection 1차 연결 성능 0.37299999967217445

$ node nodeQueryConnectionPool.js 
getConnection 1차 연결 성능 0.354500001296401

```
> 성능이 일정 수준으로 비슷하게 나옴
<br>

- 이제 wireshark를 통해 hand-shake 과정을 확인해보자

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/f5b5e439-d843-45a4-aeef-1712ff6f2ab7)
> mysql로 검색하면 필터링 가능
<br>

- node를 실행하면 위와 같이 패킷을 볼 수 있는데, 우클릭을 눌러서 더 자세한 TCP 정보 확인이 가능하다.
  - 1개의 연결당 하나의 대화만 확인 가능

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/a4d028cd-4b0a-4e5e-94e2-4a92dba648f3)
> TCP 클릭
<br>

- 첫 번째 연결 TCP 확인

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/d621ffa3-2867-4c9f-a4ec-8899441a4fa3)
<br>

- 두 번째 연결 TCP 확인

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/859a8d9f-9aea-488f-8529-b4631ecaca9d)
<br>

- pool을 사용하지 않았기 때문에 query 실행에 대해서 연결, 해제 과정을 지속적으로 반복함
  - 즉, 트래픽이 늘어날수록 hand-shake 과정이 늘어나면서 서버에 부하를 준다.
<br>
<br>

- 이제는 pool을 사용하여 hand-shake 과정을 확인해보자.
  - `conTest(true, 1)`을 주석처리하고 `conTest(fasle, {num}`) 함수를 3번 실행

```
$ node nodeQueryConnectionPool.js 
Pool 1차 연결 성능 19.19319999963045
Pool 2차 연결 성능 0.3980000000447035
Pool 3차 연결 성능 0.7635999992489815
```
> 1차 연결시에는 pool이 없기 때문에 지연, 이후 성능이 빨라짐
<br>

- wireshark 확인

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/dfcdef44-1e4c-4816-926b-30c2c3c4e449)
<br>

- TCP 대화 확인

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/98fcf4da-4409-4690-89f4-43fe9809d351)
> 3way hand-shake 발생

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/2a0827b0-8610-4c9b-aac1-93a89734b044)
> 3way hand-shake 발생

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/fc443dae-8678-4814-95b0-8d4e9e402e4d)
> 3way hand-shake 발생


<br>
<hr>
<br>

**Reference**<br>

[Jayon : Connection Pool이란?](https://steady-coding.tistory.com/564)<br>
[linked2ev : 커넥션 풀(Connection pool)이란?](https://linked2ev.github.io/spring/2019/08/14/Spring-3-%EC%BB%A4%EB%84%A5%EC%85%98-%ED%92%80%EC%9D%B4%EB%9E%80/)<br>
[gwon713 : Nodejs DB connection pool](https://velog.io/@gwon713/Nodejs-MySQL-DB-connection-pool)
