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
<br>
<br>

### 논-클러스터링 인덱스 - 인덱스 구성 후 데이터 찾는법
<img width="1258" height="734" alt="image" src="https://github.com/user-attachments/assets/d0174608-9ee6-4509-a2b9-e4f76cc8201b" />
<br>

- 데이터 조회 시
<img width="1255" height="729" alt="image" src="https://github.com/user-attachments/assets/c9a1732d-bf84-439c-9e90-2cf239a7a0e2" />
<br>
<br>

### 논-클러스터링 인덱스 특징
- 실제 데이터 페이지는 그대로

- 별도의 인덱스 페이지 생성 -> 추가 공간 필요

- 테이블당 여러 개 존재

- 리프 페이지에 실제 데이터 페이지 주소를 담고 있음
  - 클러스터링 인덱스와 함께 사용 시 : 리프 페이지에 클러스터링 인덱스가 적용된 컬럼의 실제 값 사용
<br>

- unique 제약조건 적용시 자동 생성

- 직접 index 생성 시 논-클러스터링 인덱스 생성
<br>
<hr>
<br>

## ✔️ 클러스터링 + 논-클러스터링 인덱스 동시 적용
<img width="1024" height="486" alt="image" src="https://github.com/user-attachments/assets/3f99e17d-6646-4966-bbf0-fa7030c68baa" />
<br>

- 앞서 정리한 글을 토대로 예상되는 인덱스 페이지는 어떨까?
<img width="1250" height="724" alt="image" src="https://github.com/user-attachments/assets/f714b1f1-a0f0-4471-af70-d4dc0915c4b1" />
<br>

- 위에서 예상되는 인덱스 상황은 클러스터링 인덱스 리프 페이지에 주소값을 논-클러스터링 인덱스에서 바라보고 있다.

- 그러나 클러스터링과 논-클러스터링을 동시에 적용하면 논-클러스터링은 클러스터링 인덱스의 리프 페이지에 대한 주소값이 아닌,
실제 인덱스 값을 바라본다.

<img width="1257" height="720" alt="image" src="https://github.com/user-attachments/assets/d50c21db-3ce6-4867-8027-a449d1d39c05" />
<br>

<img width="1253" height="723" alt="image" src="https://github.com/user-attachments/assets/a3de47a9-d519-4df5-abeb-283e173db509" /><br>
> 논-클러스터링 인덱스로 데이터 검색 시 클러스터링 인덱스의 id값을 가지고, 그 값을 토대로 검색을 한다.
<br>

### 클러스터링과 논-클러스터링을 동시 사용 시 주소값을 사용하지 않는 이유
- 데이터베이스는 페이지 라는 단위를 사용하여 데이터를 보관 및 정렬한다.

- 만약 논-클러스터링 인덱스가 클러스터링 인덱스를 동시에 사용하면서 주소값을 바라보면 값이 추가 될 때 마다
페이지 분할이 일어나게 된다.

- 페이지 분할이 일어나면 데이터 정렬 및 주소값 변경이 일어나기 때문에 성능 저하가 발생할 수 있다.

- 아래 사진을 보고 참고해보자.

<img width="1258" height="726" alt="image" src="https://github.com/user-attachments/assets/7d454c2f-5573-46c4-bc74-d63afaf99d24" /><br>
> 데이터가 중간에 삽입 시 페이지 분할 및 주소값 변경으로 인한 성능 저하 발생
<br>
<hr>
<br>

## ✔️ 인덱스 적용 기준
### 사용하면 좋은 경우
- 카디널리티가 높은 (중복도가 낮은) 컬럼

- WHERE, JOIN, ORDER BY 절에 자주 사용되는 컬럼
  - 인덱스는 추가 공간이 필요
  - 조건 절이 없다면 인덱스가 사용되지 않음
<br>

- INSERT, UPDATE, DELETE가 자주 발생하지 않는 컬럼

- 규모가 작지 않은 테이블
<br>
<hr>
<br>

**Reference**<br>

[우하한테크 : 라라, 제로의 데이터베이스 인덱스](https://www.youtube.com/watch?v=edpYzFgHbqs&t=891s)
