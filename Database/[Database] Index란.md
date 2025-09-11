# Index란 무엇인가?
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
<br>

- pk와 unique key를 통해 인덱스를 생성하는 법에 대해 알아보았다.

- 하지만 두 개가 같은 인덱스라고 생각하면 안되고, 2가지로 다시 분류가 된다.
  - 클러스터링 인덱스
  - 논클러스터링 인덱스
<br>

- 위에서 말한 두 가지 인덱스에 대해 좀 더 알아보자.
<br>
<hr>
<br>

## ✔️ 인덱스의 종류 : 클러스터링 인덱스와 논-클러스터링 인덱스
- 클러스터링 인덱스 : 실제 데이터와 같은 무리의 인덱스

- 논-클러스터링 인덱스 : 실제 데이터와 다른 무리의 별도 인덱스

<img width="1242" height="569" alt="image" src="https://github.com/user-attachments/assets/7e5cd38a-a71a-4c18-baf3-1efaed8e5353" /><br>
<br>

- 앞서 생성 방법으로 예시를 들었던 create 문을 다시 봐보자

```sql
CREATE TABLE member (
  id int             primary key, ---> 클러스터링 인덱스
  name varchar(255),
  email varchar(255) unique       ---> 논-클러스터링 인덱스
```
> 두 가지의 인덱스가 사실은 다른 인덱스였다.
<br>
<br>

### 클러스터링 인덱스 추가 방법
```sql
-- 방법 1
ALTER TABLE member
ADD CONSTRAINT pk_id PRIMARYT KET (id);

-- 방법 2
ALTER TABLE member MODIFY COLUMN id int NOT NULL;
ALTER TABLE member ADD CONSTRAINT nuq_id UNIQUE (id);
```
> 2번 방법은 NOT NULL과 UNIQUE를 동시에 걸어줘야 클러스터링 인덱스로 적용이 가능
<br>
<br>

### 클러스터링 인덱스 - 인덱스 구성 후 (MySQL 기준 : B-Tree 구조)
<img width="1271" height="620" alt="image" src="https://github.com/user-attachments/assets/dba37dd2-26ea-48e3-9f42-ae53d6d2d615" /><br>
<br>

- ex) `SELECT * FROM member WHERE id = 7`의 데이터 찾는 법
<img width="1263" height="629" alt="image" src="https://github.com/user-attachments/assets/b54673e1-64ec-4a71-90dd-0ed155a2d33d" />
> 루트 페이지에 주소가 담긴 리프 페이지를 찾아 그 안에서 데이터를 빠르게 찾는다.
<br>
<br>

### 클러스터링 인덱스 특징
- 실제 데이터 자체가 정렬

- 테이블당 1개만 존재 가능

- B-Tree의 리프 페이지가 데이터 페이지

- 아래의 제약조건 시 자동 생성
  - primary key (우선순위)
  - unique + not null
<br>
<br>

### 논-클러스터링 인덱스 추가 방법
<img width="1120" height="423" alt="image" src="https://github.com/user-attachments/assets/bc6163bd-a13d-4e14-8480-cd0c55f7dfcf" /><br>
<br>

```sql
-- 방법 1
ALTER TABLE member
ADD CONSTRAINT unq_name UNIQUE (name);

-- 방법 2 : UNIQUE 인덱스 -> 중복 허용 x
CREATE UNIQUE INDEX unq_idx_name
ON member (name);

-- 방법 3 : 디폴트 인덱스 -> 중복 허용
CREATE INDEX idx_name
ON member (name);
```
