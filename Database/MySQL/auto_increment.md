## auto_increment
- 시퀀스를 설정할 때, 프로시저 + 함수 생성을 통해 진행하는 방법이 있었지만 컬럼 자체에 `autu_increment` 속성을<br>
부여하면 손쉽게 시퀀스를 설정할 수 있다.

- table 생성시 시퀀스를 설정할 컬럼에 아래와 같이 설정해주면 된다.

```
CREATE TABLE tb_test (
    USER_SEQ INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    USER_ID VARCHAR(100) NOT NULL,
    USER_PW VARCHAR(100) NOT NULL,
    USER_EMAIL VARCHAR(100) NOT NULL,
    USER_PHONE VARCHAR(100) NOT NULL,
    USER_TOKEN VARCHAR(500) NOT NULL
)
```

**💡 주의 사항**
- `auto_increment` 를 설정할 컬럼은 int나 float같은 숫자값에만 사용이 가능

- primary key로 설정된 컬럼에만 사용이 가능
<br>
<hr>
<br>

### ✔ inert 실행
```
INSERT INTO tb_test
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
     null,
    "USER_ID data",
    "USER_PW data",
    "USER_EMAIL data",
    "USER_PHONE data",
    "USER_TOKEN data"
);
```
<br>
<br>

- `auto_increment`가 설정된 컬럼에는 `null`로 넣어주면 자동 증가가 된다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/49af000c-0aa2-4f27-8b15-12de5986f273)
