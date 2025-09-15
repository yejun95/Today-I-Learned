# 복합 인덱스(Composite Index)란 무엇인가?
- 데이터베이스에서 두 개 이상의 컬럼을 조합하여 생성된 인덱스

- 단일 열에 대한 인덱스와 달리, 다중 조건 검색을 최적화 시킨다.

> ❗ 더 알아보기<br>
> 결합인덱스(Joined Index) : 두 개 이상의 테이블을 조인할 때 사용하는 인덱스<br>
> 두 개 이상의 테이블에서 조인 조건으로 사용되는 컬럼을 모두 포함하는 인덱스를 말한다.
<br>
<hr>
<br>

## ✔️ 복합 인덱스 특징
- 순서 의존성 : 인덱스를 생성할 때, 지정된 열의 순서가 검색 조건에 영향을 미침
  - ex) `(A, B)` 순서로 생성된 복합 인덱스는 `A` 또는 `A, B`로 검색할 때만 인덱스가 적용됨
  - `B`만 단독으로 사용 시 인덱스가 적용되지 않음
<br>

- 범위 조건 : 범위 조건이 포함되면 열에 대한 인덱스는 효율이 떨어진다.
  - ex) `WHERE A = 10 AND B > 20`에서 `B > 20` 이후 열의 인덱스는 사용되지 않을 가능성이 존재
<br>
<hr>
<br>

## ✔️ 장점
- 검색 속도 개선

- 데이터 정렬의 효율성

- 인덱스의 용량 절감

- 쿼리 최적화
<br>
<hr>
<br>

## ✔️ 주의점
- 인덱스로 설정한 컬럼 개수가 많아질수록 인덱스의 성능이 떨어진다.

- 인덱스 생성 순서도 반드시 고려 -> 순서대로 조회하지 않으면 인덱스가 적용되지 않음

- 인덱스 생성 순서는 WHERE 절에서 먼저 사용되는 컬럼을 앞쪽에 위치시키는 것이 효율적
  - 순서 의존성을 통해 인덱스를 무조건 적용시키기 위함
<br>

- OR 조건으로 사용되는 경우 복합 인덱스를 사용하면 안됨
  - AND 조건으로 검색 시 효율적
<br>
<hr>
<br>

## ✔️ 사용방법
- 인덱스 생성은 데이터베이스마다 차이가 있지만 일반적인 절차는 아래와 같다.
  - 인덱스를 생성할 테이블과 컬럼 선택
  - 인덱스 생성할 컬럼의 순서를 결정
  - 복합 인덱스를 생성
<br>

### 복합 인덱스 생성 for MySql
```sql
CREATE INDEX index_name ON table_name (column1, column3, column2)
```
<br>

### 필요한 기능에서 쿼리문의 사용
```sql
SELECT * FROM table_name WHERE column1 = 'value1';
SELECT * FROM table_name WHERE column1 = 'value1' AND column3 = 'value3';
SELECT * FROM table_name WHERE column1 = 'value1' AND column3 = 'value3' AND column2 = 'value2';
```
<br>

### 복합 인덱스가 사용되지 않는 예시
```sql
SELECT * FROM table_name WHERE column2 = 'value2'
SELECT * FROM table_name WHERE column2 = 'value2' AND column3 = 'value3'
SELECT * FROM table_name WHERE column3 = 'value3' AND column2 = 'value2'
```
> 복합 인덱스의 첫번째 컬럼인 column1가 없기 때문에 어떤 순서로 적용하던지 복합 인덱스가 적용되지 않는다.
<br>
<hr>
<br>

**Reference**<br>

[신성훈 : 25.01.03 TIL 복합 인덱스](https://velog.io/@dotofi/25.01.03-TIL-%EB%B3%B5%ED%95%A9-%EC%9D%B8%EB%8D%B1%EC%8A%A4)<br>
[권태형 : 복합인덱스(Composite Index)](https://velog.io/@kwontae1313/%EB%B3%B5%ED%95%A9%EC%9D%B8%EB%8D%B1%EC%8A%A4)<br>
[MarrRang : DB의 결합 인덱스에 관해서](https://marrrang.tistory.com/74)
