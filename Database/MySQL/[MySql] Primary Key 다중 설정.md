## MySQL primary key 다중 설정
- Table을 정의 할 때, 특정 컬럼에 primary key를 설정하게 된다.

- 명확한 key가 있다면 1개만 지정해도 되지만, 그것이 아니라면 다중으로 지정도 가능하다.
<br>
<hr>
<br>

### ✔ primary key 설정
- 1개 설정시
```
CREATE TABLE tb_multiPK (
    USER_SEQ VARCHAR(1000) NOT NULL PRIMARY KEY,
    USER_ID VARCHAR(100) NOT NULL,
    USER_PW VARCHAR(100) NOT NULL,
    USER_EMAIL VARCHAR(100) NOT NULL,
    USER_PHONE VARCHAR(100) NOT NULL,
    USER_TOKEN VARCHAR(500) NOT NULL
)
```
<br>
<br>

- 다중 설정
  - 아래와 같이 컬럼에 직접 설정 하면 에러가 발생한다.
```
CREATE TABLE tb_multiPK (
    USER_SEQ VARCHAR(1000) NOT NULL PRIMARY KEY,
    USER_ID VARCHAR(100) NOT NULL PRIMARY KEY,
    USER_PW VARCHAR(100) NOT NULL,
    USER_EMAIL VARCHAR(100) NOT NULL,
    USER_PHONE VARCHAR(100) NOT NULL,
    USER_TOKEN VARCHAR(500) NOT NULL PRIMARY KEY
)
```

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/5023fdd8-d2f8-491b-a8ba-63eae6e4d1e9)
<br>
<br>

- 스크립트 하단에 따로 작성을 하면 에러 없이 정상 진행 된다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/85d0167f-ff8f-4edd-9eaf-2fee10fda50e)
<br>
<hr>
<br>

### ✔ 기존 컬럼에 PK 추가하기
- 테이블을 생성 할 때, PK를 1개만 줬더라면 `ALTER` 명령어를 통해 PK 지정이 가능하다.
  - `ALTER TABLE 테이블명 ADD PRIMARY KEY (pk로 지정할 컬럼명);`
<br>

- 반대로 이미 지정된 PK를 삭제할 때
  - table의 모든 PK가 해제된다.
  - `ALTER TABLE 테이블명 DROP PRIMARY KEY;`
<br>
<hr>
<br>

**Reference**<br>

[AyoteraLab 지식 스케치 : Mysql에서 multi PK (다중 기본키) 설정하기](https://ayoteralab.tistory.com/entry/Mysql%EC%97%90%EC%84%9C-multi-PK-%EB%8B%A4%EC%A4%91-%EA%B8%B0%EB%B3%B8%ED%82%A4-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0)
