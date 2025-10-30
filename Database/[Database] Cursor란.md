# 데이터베이스의 커서란?
- SQL의 커서는 결과 집합을 한 행씩 순회하면서 처리하는 메커니즘이다.

- 다른 프로그래밍 언어의 반복자(iterator)와 같이 레코드 검색, 추가, 제거와 같은 처리를 순회와 함께할 때 용이하다.

- 프로시저에서 커서를 사용하면 결과 집합(데이터 행 집합)을 정의하고 행 단위로 복잡한 논리를 수행할 수 있다.

- 즉, 일반적인 `SELECT`는 결과를 한 번에 반환하지만, 커서는 결과를 한 행씩 가져와 처리할 수 있게 한다.
<br>
<hr>
<br>

## ✔️ 사용 이유
- 행 단위 처리: 각 행마다 다른 로직 적용

- 대량 데이터 처리: 한 번에 메모리에 올리기 어려운 경우

- 복잡한 비즈니스 로직: 행별 조건부 처리
<br>
<hr>
<br>

## ✔️ 사례
### 기본 구조
```sql
-- 1. 커서 선언
DECLARE cursor_name CURSOR FOR
SELECT column1, column2
FROM table_name
WHERE condition;

-- 2. 커서 열기
OPEN cursor_name;

-- 3. 데이터 가져오기 (FETCH)
FETCH NEXT FROM cursor_name INTO @var1, @var2;

-- 루프로 처리
WHILE @@FETCH_STATUS = 0
BEGIN
    -- 각 행에 대한 처리 로직
    -- ...
    
    -- 다음 행 가져오기
    FETCH NEXT FROM cursor_name INTO @var1, @var2;
END

-- 4. 커서 닫기 및 해제
CLOSE cursor_name;
DEALLOCATE cursor_name;
```
<br>

### 실제 예제
```sql
-- 직원들의 급여를 10% 인상하고 로그를 남기는 경우
DECLARE @EmployeeID INT;
DECLARE @Salary DECIMAL(10,2);
DECLARE @NewSalary DECIMAL(10,2);

DECLARE salary_cursor CURSOR FOR
SELECT EmployeeID, Salary
FROM Employees
WHERE DepartmentID = 1;

OPEN salary_cursor;

FETCH NEXT FROM salary_cursor INTO @EmployeeID, @Salary;

WHILE @@FETCH_STATUS = 0
BEGIN
    -- 급여 계산
    SET @NewSalary = @Salary * 1.10;
    
    -- 업데이트
    UPDATE Employees
    SET Salary = @NewSalary
    WHERE EmployeeID = @EmployeeID;
    
    -- 로그 기록
    INSERT INTO SalaryChangeLog (EmployeeID, OldSalary, NewSalary, ChangeDate)
    VALUES (@EmployeeID, @Salary, @NewSalary, GETDATE());
    
    -- 다음 행 가져오기
    FETCH NEXT FROM salary_cursor INTO @EmployeeID, @Salary;
END

CLOSE salary_cursor;
DEALLOCATE salary_cursor;
```
<br>

### 주의 사항
- 성능 문제 이슈 가능성
  - 보통 셋 기반 작업보다 느림
  - 필요한 경우에만 사용
<br>
<hr>
<br>

## ✔️ 그러면 언제 사용해야 할까?
### 사용해야 할 때:
- 각 행마다 복잡한 비즈니스 로직 처리

- 다른 테이블과의 복잡한 상호작용

- 행별 조건부 처리
<br>

### 사용하지 말아야 할 때:
- 단순 업데이트/삭제(셋 기반 작업으로 대체)

- 대량 데이터 처리(배치 처리 고려)

- 일반 조회
<br>
<hr>
<br>

## ✔️ 정리
- 커서는 행 단위 처리용 도구

- 성능을 고려해 필요한 경우에만 사용

- 가능하면 임시테이블을 생성하여 진행하는 것이 효율적
