# Statement보다 PreparedStatement를 사용해야 하는 이유
- Statment 종류에는 Statement, PreparedStatement, CallableStatement 3가지가 존재

- CallableStatement는 PL/SQL문을 호출할 때 사용한다고 했지만 성능상 이슈로 인해 거의 사용하지 않기에 Statement나 PreparedStatement를 사용

- 그런데 실무 환경에서는 Statement는 쓰지 않고 PreparedStatement만 사용하는데 그 이유를 설명해보려한다.
<br>
<hr>
<br>

## ✔️ Statement, PreparedStatement 동작 방식
- Statement와 PreparedStatement는 아래와 같은 공통 실행 과정을 거친다.

![image](https://github.com/user-attachments/assets/1798d27c-ac16-4207-9645-d93b452e8df8)
<br>

- 구문 분석 (Parsing) 및 정규화 (Normalization)
  - Query 문법 확인 및 데이터베이스, 테이블 존재여부 확인
<br>

- 컴파일 (Compliation)
  - Query 컴파일
<br>

- Query 최적화 (Query Optimization Phase)
  - Query 실행 방법의 최적 계획 선택
<br>

- 캐시 (Cache)
  - Query 최적화 단계에서 선택된 계획이 캐시에 저장되어 동일한 Query 실행 시, 1 ~ 3단계를 실행하지 않고 캐시를 통해 찾음
<br>

- 실행 (Execution Phase)
  - Query가 실행된 값이 담긴 객체 (ResultSet)를 사용자에게 반환
<br>
<hr>
<br>

## ✔️ Statement와 PreparedStatement의 차이점
### 캐시 사용 유무 (재사용성)
- Statement는 매번 Query 실행 시 마다, 처음부터 끝까지의 과정을 반복

- PreparedStatement는 최초 Query 실행 시, 처음부터 끝까지 진행 후 두 번째 Query 실행부터 1~3번 까지의 과정을 생략

- 그러나 Statement 방식도 캐시를 사용한다.
  - 첫 수행한 Query와 완전히 일치하는 Query를 요청하는 경우에만 캐싱 데이터 활용 가능
<br>

```java
[Statement]
Connection conn = DriverManager.getConnection(url, id, pwd);
Statement stmt = conn.createStatement();
stmt.executeUpdate("Update Member Set Name = " + "홍길동" Where id = " + 10);
stmt.executeUpdate("Update Member Set Name = " + "김길동" Where id = " + 11;
```
> 위 쿼리와 같이 조금이라도 쿼리 내용이 달라지면 Statement에서는 캐싱을 사용할 수 없다.
<br>

- 그에 비해 PreparedStatement 방식은  placeHolder(?)를 이용하여 파라미터를 바인딩하기 때문에 기존 컴파일된 Query 재사용 가능

```java
[PreparedStatement]
Connection conn = DriverManager.getConnection(url, id, pwd);
String sql = "Update Member Set Name = ? Where id = ?";
PreparedStatement pstmt = conn.prepareStatement(sql);
pstmt.setString(1, "홍길동");
pstmt.setInt(2, 10);
pstmt.executeUpdate();

pstmt.setString(1, "김길동");
pstmt.setInt(2, 11);
pstmt.executeUpdate();
```
> String sql = "Update Member Set Name = ? Where id = ?" 부분을 공통적으로 쓰고 있다.
<br>

- 따라서, Prepared Statement 방식을 사용하면 DB의 부하를 낮추고 속도를 더 높일 수 있다.
<br>

### SQL Injection 방지
- PreparedStatement를 사용하면 SQL Injection 공격 방지 가능

- 아래 코드 참조

```java
[기존 Statement 방식]
Connection conn = DriverManager.getConnection(url, id, pwd);
Statement stmt = conn.createStatement();
stmt.executeQuery("Select * from Member Where id = " + id + " and pwd = " + pwd);

→ 로그인 시, 사용자에게서 입력받은 값을 Where 조건절에 넣어주어 조회하는 Query이지만 만약, 다음과 같이 공격자가 파라미터 값을 악의적으로 조작해서 보낼 수도 있다.

[사용자에게서 입력받은 값]
id = test123
pwd = 1234’ or 1=1

Query = Select * from Member Where id = ‘test123’ and pwd = ‘1234’ OR ‘1’ = ‘1’;

→ 이처럼 공격자가 악의적으로 파라미터 값을 조작하여 “OR 1=1” 조건을 추가하는 방법으로 사용자 정보를 탈취할 수 있다.

[PreparedStatment 방식]
Connection conn = DriverManager.getConnection(url, id, pwd);
String sql = "Select * from Member Where id = ? and pwd = ?";
PreparedStatement pstmt = conn.prepareStatement(sql);
pstmt.setString(1, id);
pstmt.setString(2, pwd);
pstmt.executeQuery();

→ PreparedStatment 방식은 사용자에게서 받은 파라미터 값이 악의적으로 조작되었다 하더라도 파라미터 바인딩을 통해 SQL Injection을 방지할 수 있다.

→ 정확히는 pstmt.setString() 메소드 내부에서 사용되는 javaEncode() 메소드가 SQL Injection을 방지해주는데 내부 구현이 어떻게 되어있는지 살펴보도록 하자.
```
<br>

```java
[javaEncode() 메소드]
public static void javaEncode(String s, StringBuilder buff, boolean forSQL) {
    int length = s.length();

    for (int i = 0, i < length; i++) {
        char c = s.charAt(i);

        switch (c) { 
            case ‘\t’:
                buff.append(“\\t”);
                break;
            case ‘\n’:
                buff.append(“\\n”);
                break;
            case ‘\f’:
                buff.append(“\\f”);
                break;
            case ‘“’;
                buff.append(‘\’’);
                break;
                ...
        }
    }
}

→ 입력 문자열에 포함된 특수 문자(예: \t, \n, ', ")를 이스케이프 처리(예: \', \\)함으로써, SQL 구문이 깨지지 않도록 보호
```
> 즉, 사용자가 ' OR 1=1 -- 같은 문자열을 주더라도, 그 안의 '을 \' 또는 안전한 문자로 바꾸면 SQL 문법이 깨지고 의도된 쿼리가 되지 않게 함
<br>
<hr>
<br>

## ✔️ 정리
### SQL Injection 방지
- PreparedStatement는 파라미터를 자동으로 이스케이프 처리해주기 때문에, 사용자가 입력한 값이 SQL 쿼리 구조에 영향을 미치지 않도록 한다.

- 반면, Statement는 문자열을 직접 조합하므로, 악의적인 입력(' OR 1=1 --)이 쿼리 실행에 영향을 줄 수 있다.

예시:
```java
// Statement (취약)
Statement stmt = conn.createStatement();
ResultSet rs = stmt.executeQuery("SELECT * FROM users WHERE name = '" + input + "'");

// PreparedStatement (안전)
PreparedStatement pstmt = conn.prepareStatement("SELECT * FROM users WHERE name = ?");
pstmt.setString(1, input);
ResultSet rs = pstmt.executeQuery();
```
<br>
<br>

### 컴파일된 쿼리 재사용 (성능 향상)
- PreparedStatement는 SQL을 한 번 파싱하고(컴파일) 이후에는 바인딩 값만 바꿔서 재사용 가능

- DB가 동일한 쿼리 구조를 반복적으로 실행할 때, 쿼리 캐시를 활용할 수 있어 성능이 향상
<br>
<br>

### 가독성과 유지보수성
- PreparedStatement는 SQL과 값 세팅이 분리되어 있어서 코드가 더 읽기 쉽고 유지보수가 쉽다.

- Statement는 문자열 연결이 복잡하고, 큰 쿼리는 가독성이 매우 떨어진다.
<br>
<br>

### 자동 타입 매핑
- PreparedStatement는 setInt, setString, setDate 등으로 타입에 맞게 바인딩되므로, 타입 오류 가능성이 줄어든다.
<br>
<br>

### 요약 비교
| 항목               | Statement  | PreparedStatement |
| ---------------- | ---------- | ----------------- |
| SQL Injection 방지 | ❌          | ✅                 |
| 쿼리 캐시 활용         | ❌          | ✅                 |
| 성능               | 느림 (매번 파싱) | 빠름 (쿼리 재사용)       |
| 가독성              | 낮음         | 높음                |
| 타입 안전성           | 낮음         | 높음                |
<br>
<hr>
<br>

**Refernce**<br>

[KR_DEV : Statement보다 PreparedStatement를 사용해야 하는 이유](https://shuu.tistory.com/129)<br>
[GonnabeAlright : [DataBase] Statement와 Prepared Statement 차이점](https://velog.io/@ragnarok_code/DataBase-Statement%EC%99%80-Prepared-Statement-%EC%B0%A8%EC%9D%B4%EC%A0%90)<br>
[장화평 : Statement VS PreparedStatement 차이점](https://hpjang.tistory.com/3)
