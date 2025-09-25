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
BEGIN TRANSACTION;t
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
<br>

### 인덱스 업데이트 최적화
- 개별 insert의 인덱스 처리
  - 각 insert 마다
  - B-Tree 인덱스 구조 재구성
  - 인덱스 페이지 분할 가능성
  - 인덱스 정렬 작업
  - 디스크 I/O 발생
<br>

- 벌크 insert의 인덱스 처리
  - 인덱스 업데이트를 배치로 처리
  - 인덱스 페이지 분할 최소화
  - 정렬 작업을 한 번에 수행
  - 디스크 I/O 최적화
<br>
<hr>
<br>

## ✔️ 실제 테스트 진행
### 테스트 시나리오
- Database : MySQL (Windows)
- 테스트 데이터 생성 : 파이썬
- 시나리오 : 
  - 개별 insert 10000개
  - 벌크 insert 10000개
  - 1000개씩 배치 insert
<br>

### 테스트 데이터 파일(.sql) 생성
```python
import random

# 영문 이름 리스트
names = ['John', 'Jane', 'Mike', 'Sarah', 'David', 'Lisa', 'Tom', 'Amy', 'Chris', 'Emma',
         'Alex', 'Kate', 'Ben', 'Anna', 'Sam', 'Mary', 'Dan', 'Lucy', 'Paul', 'Grace']

# 나이 범위
ages = list(range(18, 81))

def generate_test_data():
    data = []
    for i in range(1, 10001):
        name = random.choice(names) + str(i)
        age = str(random.choice(ages))
        data.append((i, name, age))
    return data

def generate_individual_sql(data):
    sql_statements = []
    for id, name, age in data:
        sql = f"INSERT INTO user (id, name, age) VALUES ({id}, '{name}', '{age}');"
        sql_statements.append(sql)
    return sql_statements

def generate_bulk_sql(data):
    values = []
    for id, name, age in data:
        values.append(f"({id}, '{name}', '{age}')")
    return "INSERT INTO user (id, name, age) VALUES\n" + ",\n".join(values) + ";"

def generate_batch_sql(data, batch_size=1000):
    batches = []
    for i in range(0, len(data), batch_size):
        batch_data = data[i:i+batch_size]
        values = []
        for id, name, age in batch_data:
            values.append(f"({id}, '{name}', '{age}')")
        batch_sql = "INSERT INTO user (id, name, age) VALUES\n" + ",\n".join(values) + ";"
        batches.append(batch_sql)
    return batches

# 테스트 데이터 생성
print("영문 테스트 데이터 생성 중...")
data = generate_test_data()

# 1. 개별 INSERT 파일 생성
print("개별 INSERT 파일 생성 중...")
with open('individual_inserts_eng.sql', 'w', encoding='utf-8') as f:
    f.write("-- 개별 INSERT 성능 테스트 (영문)\n")
    f.write("TRUNCATE TABLE user;\n\n")
    f.write("SET @start_time = NOW(6);\n\n")
    
    statements = generate_individual_sql(data)
    for statement in statements:
        f.write(statement + '\n')
    
    f.write("\nSET @end_time = NOW(6);\n")
    f.write("SET @duration = TIMESTAMPDIFF(MICROSECOND, @start_time, @end_time) / 1000000;\n\n")
    f.write("SELECT 'Individual INSERT' as test_type, COUNT(*) as total_records, @duration as duration_seconds, ROUND(COUNT(*) / @duration, 2) as records_per_second;\n")

# 2. 벌크 INSERT 파일 생성
print("벌크 INSERT 파일 생성 중...")
with open('bulk_insert_eng.sql', 'w', encoding='utf-8') as f:
    f.write("-- 벌크 INSERT 성능 테스트 (영문)\n")
    f.write("TRUNCATE TABLE user;\n\n")
    f.write("SET @start_time = NOW(6);\n\n")
    
    f.write(generate_bulk_sql(data))
    f.write("\n\n")
    
    f.write("SET @end_time = NOW(6);\n")
    f.write("SET @duration = TIMESTAMPDIFF(MICROSECOND, @start_time, @end_time) / 1000000;\n\n")
    f.write("SELECT 'Bulk INSERT' as test_type, COUNT(*) as total_records, @duration as duration_seconds, ROUND(COUNT(*) / @duration, 2) as records_per_second;\n")

# 3. 배치 INSERT 파일 생성
print("배치 INSERT 파일 생성 중...")
with open('batch_insert_eng.sql', 'w', encoding='utf-8') as f:
    f.write("-- 배치 INSERT 성능 테스트 (영문, 1000개씩)\n")
    f.write("TRUNCATE TABLE user;\n\n")
    f.write("SET @start_time = NOW(6);\n\n")
    
    batches = generate_batch_sql(data, 1000)
    for batch in batches:
        f.write(batch + '\n\n')
    
    f.write("SET @end_time = NOW(6);\n")
    f.write("SET @duration = TIMESTAMPDIFF(MICROSECOND, @start_time, @end_time) / 1000000;\n\n")
    f.write("SELECT 'Batch INSERT (1000씩)' as test_type, COUNT(*) as total_records, @duration as duration_seconds, ROUND(COUNT(*) / @duration, 2) as records_per_second;\n")

print("영문 테스트 파일이 생성되었습니다!")
print("- individual_inserts_eng.sql: 개별 INSERT 테스트")
print("- bulk_insert_eng.sql: 벌크 INSERT 테스트")
print("- batch_insert_eng.sql: 배치 INSERT 테스트")
```
> 파일명 : complete_test.py
<br>
<br>

### 테스트를 위한 MySql CLI 사용
- 필자는 git bash에서 mysql 명령어를 사용해 `.sql` 파일을 실행하였음
<br>

**개별 insert**
```sql
$ mysql -u root -p practice < individual_inserts_eng.sql
Enter password: ****
test_type       total_records   duration_seconds        records_per_second
Individual INSERT       1       9.996689000     0.10
```
<br>
<br>

**벌크 insert**
```sql
$ mysql -u root -p practice < bulk_insert_eng.sql
Enter password: ****
test_type       total_records   duration_seconds        records_per_second
Bulk INSERT     1       0.095561000     10.46
```
<br>
<br>

**1000개씩 배치를 통한 벌크 insert**
```sql
$ mysql -u root -p practice < batch_insert_eng.sql
Enter password: ****
test_type       total_records   duration_seconds        records_per_second
Batch INSERT (1000?©)   1       0.104843000     9.54
```
<br>
<hr>
<br>

## ✔️ 정리
### 성능 테스트 결과
| 테스트 유형 | 총 레코드 수 | 소요 시간 (초) | 초당 처리 레코드 수 | 성능 비율 |
|------------|-------------|---------------|-------------------|----------|
| **개별 INSERT** | 10000 | 9.99 | 0.10 | 1x (기준) |
| **벌크 INSERT** | 10000 | 0.09 | 10.46 | **104.6x** |
| **배치 INSERT (1000씩)** | 10000 | 0.10 | 9.54 | **95.4x** |
<br>
<br>

### 성능 분석
- 벌크 INSERT가 가장 빠름
  - **104.6배** 빠른 성능
  - 10,000개 데이터를 **0.096초**에 처리
  - 초당 **10.46개** 레코드 처리
<br>

- 배치 INSERT (1000개씩)
  - **95.4배** 빠른 성능  
  - 10,000개 데이터를 **0.105초**에 처리
  - 초당 **9.54개** 레코드 처리
<br>

- 개별 INSERT (비효율적)
  - **9.997초** 소요
  - 초당 **0.10개** 레코드 처리
  - 벌크 INSERT 대비 **100배 이상 느림**
<br>

### 결론
- 벌크 INSERT의 장점
  - **네트워크 오버헤드 최소화** - 1번의 통신으로 모든 데이터 전송
  - **SQL 파싱 비용 절약** - 1번의 파싱으로 모든 데이터 처리
  - **트랜잭션 효율성** - 1번의 트랜잭션으로 모든 데이터 처리
  - **인덱스 업데이트 최적화** - 배치로 인덱스 업데이트
<br>

- 권장사항
  - **대용량 데이터 삽입 시**: 벌크 INSERT 사용
  - **메모리 제약이 있는 경우**: 배치 INSERT (1000-5000개씩)
  - **개별 INSERT는 피하기**: 성능상 매우 비효율적
