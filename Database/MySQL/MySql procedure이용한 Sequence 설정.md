## MySQL Sequence 설정
### ✔ 시퀀스(Sequence)란?
-  유일(UNIQUE)한 값을 생성해준다.

- 시퀀스를 생성하면 기본키와 같이 순차적으로 증가하는 컬럼을 자동적으로 생성 할 수 있다.

- insert를 통해 새로운 값이 들어오면 자동적으로 숫자가 증가하는 것이다 .
<br>
<hr>
<br>

### ✔ MySQL 시퀀스 설정
- MySQL 에서는 오라클처럼 별도의 시퀀스 기능이 존재하지 않는다.

- 그러나 사용하려면 다음과 같은 절차를 따라하면 가능하다. 
  - 데이터를 저장할 테이블 생성
  - 시퀀스를 사용할 테이블 생성
  - 시퀀스를 생성할 프로시저 생성
  - 생성한 시퀀스의 다음 값을 가져오는 함수 생성
  - 시퀀스를 생성할 프로시저 실행
  - 최종적으로 nextval 사용 
  - [프로시저란?](https://johoonday.tistory.com/204)
 <br>

### ✔ tb_user 테이블 생성 
- 값을 저장할 테이블 생성

```
CREATE TABLE tb_user (
    USER_SEQ VARCHAR(1000) NOT NULL,
    USER_ID VARCHAR(100) NOT NULL,
    USER_PW VARCHAR(100) NOT NULL,
    USER_EMAIL VARCHAR(100) NOT NULL,
    USER_PHONE VARCHAR(100) NOT NULL,
    USER_TOKEN VARCHAR(500) NOT NULL
)
```

### ✔ 시퀀스를 사용할 테이블 생성
```
create TABLE tb_user_sequences(
    name varchar(32),
    currval BIGINT unsigned
)
engine = innoDB;
```

- 시퀀스로 사용할 테이블을 생성한다.

- name = 시퀀스명

-  currval = 순서

- Ex) name = 'Test', currval = 1

- InnoDB는 MySQL에서 기본적으로 제공되는 트랜잭션 지원 및 격리 수준이 높은 스토리지 엔진 중 하나
<br>
<hr>
<br>

### ✔ 시퀀스를 생성할 프로시저 생성
```
-- 시퀀스로 사용할 프로시저 생성
-- 'IN' 으로 시퀀스 명을 받음
-- call [프로시저명]('[시퀀스명]')
delimiter $$
    create procedure `create_sequence` (IN the_name text)
    modifies sql data
    deterministic
    begin
        delete from tb_user_sequences where name = the_name;
        insert into tb_user_sequences values(the_name, 0);
    end;
```

- 이미 생성되어진 시퀀스가 있으면 지우고 새로운 시퀀스를 생성한다.

- create_sequence라는 저장 프로시저를 생성하며, 이 저장 프로시저는 하나의 매개변수 the_name을 받는다.

-  modifies sql data :  이 저장 프로시저는 SQL 데이터를 수정할 것임을 나타낸다.
다시 말해, 데이터베이스의 상태를 변경할 수 있는 프로시저이다.

- deterministic : 이 저장 프로시저는 결정론적이라는 의미이다. 
즉, 동일한 입력에 대해 항상 동일한 결과를 생성한다는 것을 나타낸다.
이는 저장 프로시저의 성능 최적화와 관련이 있다.
<br>
<br>

**생성한 프로시저 목록 확인 방법**
`mysql> show procedure status `

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/148a3162-657d-4584-8867-e649ddfd85ff)
<br>
<br>

**프로시저 스크립트 확인방법**
`mysql> show create procedure 프로시저이름`

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/6969c8e0-afd8-4ab8-bef8-9f2e3e431f7b)
<br>
<hr>
<br>

### ✔  생성한 시퀀스의 다음 값을 가져오는 함수 생성
```
delimiter $$
    create function `nextseq` (the_name VARCHAR(32))
    RETURNS BIGINT unsigned
    MODIFIES SQL DATA
    Deterministic
    begin
        declare ret BIGINT unsigned;
        update tb_user_sequences set currval = currval +1 where name = the_name;
        select currval into ret from tb_user_sequences where name = the_name limit 1;
        return ret;
    end;
```

**생성한 function 확인**
`show function status`

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/6865ff04-8be5-4d28-9466-74191e062838)
<br>
<br>

**특정 FUNCTION 생성문 확인**
`show create function FUNCTION명;

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/2c9c8e3e-a8e9-44d8-bf09-61dd260f9bd3)
<br>
<br>

- 💡 `nextval`이라는 이름은 native function name 이기 때문에 warning 발생하며 실행 불가능함

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/c7c1a3cb-d340-4f4b-b894-e0a6cd8c1ca4)
<br>
<hr>
<br>

### ✔ 시퀀스를 생성할 프로시저 실행
```
call create_sequence('seqnumber');   -- 'seqnumber' 라는 이름을 가진 시퀀스 생성
```

- 프로시저를 호출해서 seqnumber 라는 이름의 시퀀스를 생성한다. 

- seqnumber 시퀀스에 0 값이 할당된다.
<br>
<br>

**Error code: 1175**
- UPDATE 에 대해서 안전 모드로 되어있어서 SAFE MODE 를 꺼주면 된다.

- `SET SQL_SAFE_UPDATES = 0;`

- 이후 스크립트 실행 완료

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/a16ba4ad-abd1-48b4-8deb-7e8b5fa3ff51)
<br>
<hr>
<br>

### ✔  최종적으로 nextval 사용 
```
SELECT nextval('seqnumber') FROM DUAL
```

- 상단 스크립트 실행시 함수가 작동되면서 seqnumber의 currval 값이 증가된 것을 볼 수 있다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/ec28c8f2-21f8-46c7-8dc3-820ed71954b3)
<br>
<br>

- [DUAL 이란?](https://harrydony.tistory.com/887)

- 이제 insert sql문을 사용할 때, nextval 함수를 사용하면 된다.
<br>
<br>

**사용 예제**
```
INSERT INTO tb_user
(
    USER_SEQ,
    USER_ID,
    USER_PW,
    USER_EMAIL,
    USER_PHONE,
    USER_TOKEN
)
VALUES
(
     (SELECT nextseq('seqnumber') FROM DUAL),
    'USER_ID data',
    'USER_PW data',
    'USER_EMAIL data',
    'USER_PHONE data',
    'USER_TOKEN data'
);
```

- USER_SEQ에 `seqnumber` 의 시퀀스 값이 적용된다.
  - 1번 시퀀스는 가데이터를 수기로 넣은것이니 무시하면됨

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/2af36e94-bd14-4100-a6e7-5b2674b802eb)
<br>
<hr>
<br>

**Referencce**<br>

[프라우딘 : MySql에서 Sequence 기능을 사용하는 방법](https://proudin.tistory.com/entry/MySql%EC%97%90%EC%84%9C-Sequence-%EA%B8%B0%EB%8A%A5%EC%9D%84-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95)<br>
[실수로포스팅해버렸다 : [MySQL] 프로시저, 뷰 확인 방법](https://iwantadmin.tistory.com/137)<br>
[[ENSHA's CODII : [MYSQL] FUNCTION, PROCEDURE, TABLE, VIEW 정보 확인](https://ensha.tistory.com/entry/MYSQL-FUNCTION-PROCEDURE-TABLE-VIEW-%EC%A0%95%EB%B3%B4-%ED%99%95%EC%9D%B8)<br>


