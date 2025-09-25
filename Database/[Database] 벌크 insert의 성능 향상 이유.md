# 벌크 insert의 성능 향상 이유
- 보통 클라이언트에서 보낸 배열 정보를 통해 다건의 insert를 하는 경우 아래와 같이 진행한다.
```sql
-- 1000개 데이터를 개별로 삽입하는 경우
INSERT INTO users (name, email) VALUES ('김철수', 'kim@example.com');
INSERT INTO users (name, email) VALUES ('이영희', 'lee@example.com');
-- ... 998번 더 반복
```
<br>

- 데이터가 많지 않으면 위와 같이 insert를 해도 DB 성능에 영향을 주지 않지만, 만약 만개, 십만개를 insert를 위와 같이 하면 어떻게 될까?

- 결과는 insert 하는 시간이 상당히 오래 걸리므로 브라우저에서 유저 경험이 좋지 않게 된다.
  - 비동기 처리를 통해 insert를 하는 동안 다른 작업을 하게 만들 수는 있지만 즉각적인 insert가 되지 않으면 유저는 더 이상 사용하지 않는다.
  - 물론 Queue에 넣어 두고 배치를 통해서 insert를 하는 방식도 있겠지만, 지금은 즉시 insert 되는 상황만 가정하여 설명하겠다.
<br>
<hr>
<br>

## ✔️ 벌크 insert의 성능 향상 이유
### 1번의 네트워크 통신으로 모든 데이터 전송
- 개별 insert는 모든 insert 마다 네트워크 왕복(Round Trip)발생
  
- ex) 1000개 insert = 1000번의 네트워크 통신

- 네트워크 지연시간 x 1 = 최소 지연시간
<br>
<br>

### SQL 파싱 및 최적화 비용 절약
- 개별 insert의 처리 과정
  - SQL 문법 분석 (파싱) x 1000
  - 실행 계획 수립 x 1000
  - 인덱스 업데이트 계획 x 1000
  - 메모리 할당 x 1000
  - 트랜잭션 로그 기록 x 1000
<br>

- 벌크 insert의 처리 과정
  - SQL 문법 분석 (파싱) x 1
  - 실행 계획 수립 x 1
  - 인덱스 업데이트 계획 x 1 (배치 처리)
  - 메모리 할당 x 1 (대용량)
  - 트랜잭션 로그 기록 x 1 (배치)
<br>
<br>

### 트랜잭션 처리 효율성
- 개별 insert의 트랜잭션 처리
```sql
BEGIN TRANSACTION;
INSERT INTO users (name, email) VALUES ('김철수', 'kim@example.com');
COMMIT;  -- 트랜잭션 종료

BEGIN TRANSACTION;
INSERT INTO users (name, email) VALUES ('이영희', 'lee@example.com');
COMMIT;  -- 트랜잭션 종료
-- ... 998번 더 반복
```
> 각 insert마다 트랜잭션 시작/종료<br>
> 트랜잭션 로그 디스크 I/O x 1000<br>
> 락(Lock) 획득/해제 x 1000
<br>

- 벌크 insert의 트랜잭션 처리
```sql
BEGIN TRANSACTION;
INSERT INTO users (name, email) VALUES 
('김철수', 'kim@example.com'),
('이영희', 'lee@example.com'),
-- ... 998개 더
('박민수', 'park@example.com');
COMMIT;  -- 한 번만 트랜잭션 종료
```
> 한 번의 트랜잭션으로 모든 데이터 처리<br>
> 트랜잭션 로그 디스크 I/O 최소화<br>
> 락 획득/해제 횟수 최소화
