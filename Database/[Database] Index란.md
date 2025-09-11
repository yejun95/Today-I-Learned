# index란 무엇인가?
- index는 색인이라고도 하며, 원하는 값을 빠르게 찾기 위해 설정하는 것.

- 즉, 데이터베이스 테이블에 대한 검색 성능을 향상 시키는 자료구조이다.

- 주로 인덱스가 적용된 대상을 `where` 절로 검색하여 성능을 향상시킨다.

- 한마디로 데이터가 특정 기준으로 정렬되어 있다면 검색을 빠르게 할 수 있다.
<br>
<hr>
<br>

## ✔️ 사용법
```sql
SELECT *
FROM member
WHERE email = 'asdf123@gmail.com'
```
> 인덱스가 적용된 대상을(email로 정렬된 데이터)<br>
> 위 쿼리와 같이 WHERE 절을 사용하여 특정 키 값을 조회한다.
<br>

- 위에서 키 값을 조회한다고 되어 있는데, 그렇다면 키 값이란 어떠한 정의를 말하는 것인가?
  - primary key
  - unique key
  - 위 두개와 같이 중복되지 않는 키를 말한다.
  - 그러나 특정 키 값이 아니더라도 인덱스를 적용할 수 있는데, 해당 내용은 더 아래에서 설명하겠다.
<br>

### 생성 방법
```sql
CREATE TABLE member (
  id int             primary key,
  name varchar(255),
  email varchar(255) unique
)
```
> pk와 unique 모두 인덱스가 적용됨
