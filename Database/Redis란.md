## Redis란 무엇인가?
![image](https://github.com/user-attachments/assets/37e466ed-778f-4eea-9346-f7dfbe385d76)
<br>

- Redis란 Key, Value 구조의 비정형 데이터를 저장하고 관리하기 위한 오픈 소스 기반의 비관계형 데이터 베이스 관리 시스템 (DBMS)이다.

- 데이터베이스, 캐시, 메세지 브로커로 사용되며 인메모리 데이터 구조를 가진 저장소이다.
<br>

### 인메모리란?
- 컴퓨터의 주기억장치인 RAM에 데이터를 올려서 사용하는 방법

- RAM에 데이터를 저장하게 되면 메모리 내부에서 처리가 되므로 데이터를 저장/조회할 때<br>
하드디스크를 오고가는 과정을 거치지 않아도 되어 속도가 빠름

- 그러나 서버의 메모리 용량을 초과하는 데이터를 처리할 경우, RAM의 특성인 휘발성에 따라 데이터가 유실될 수 있음
<br>
<hr>
<br>

## ✔️ 사용 이유
- 기존에 RDB도 있고, Redis와 동일하게 Key, Value 구조를 가진 NoSQL도 존재하는데, 왜 Redis를 사용하는 걸까?

- DB는 데이터를 디스크에 직접 저장(write)하기 때문에 서버에 문제가 발생하여 다운되더라도 데이터가 손실되지 않는다.

- 매번 디스크에 접근해야하기 때문에 사용자가 많아질 수록 부하가 많아져서 느려질 수 있어서 캐시 서버를 도입하여 사용해야한다.

- 이 캐시 서버로 이용할 수 있는 것이 바로 Redis이다.

- 즉, 부하 분산을 피하고 데이터를 빠르게 조회하기 위해 캐싱을 하는 것이 Redis이다.
<br>

### 캐싱을 하기 때문에
- 같은 요청이 여러번 들어올 때 Redis를 사용함으로써 매번 DB를 거치지 않고 캐시 서버에서 저장해놨던 값을 바로 가져와<br>
DB의 부하를 줄이고 서비스의 속도도 느려지지 않게 할 수 있다.
<br>
<hr>
<br>

## ✔️ Redis의 특징
- Key, Value 구조

- 빠른 처리 속도 - 디스크가 아닌 메모리에서 데이터를 처리하기 때문

- Data Type(Collection)을 지원 - Key의 다양한 형태

![image](https://github.com/user-attachments/assets/dabc0838-c7a7-4ed6-bc33-b8a6700d562b)
<br>

- Single Threaded
  - 한 번에 하나의 명령만 처리 가능
  - 그렇기 때문에 중간에 처리 시간이 긴 명령어가 들어오면 그 뒤에 명령어들은 모두 앞에 있는 명령어가 처리될 때까지 대기 필요
  - 하지만 get, set 명령어의 경우 초당 10만 개 이상 처리할 수 있을 만큼 빠르다.
<br>
<hr>
<br>

## ✔️ 영속성
- 기존에 Redis는 disk에 저장하지 않고, RAM에 데이터를 올리는 인메모리 구조이다.

- 하지만 지속성을 보장하기 위해 데이터를 disk에도 저장할 수 있다.

- 그러면 서버가 내려가더라도 disk에 저장된 데이터를 읽어서 메모리에 로딩을 할 수 있다.
<br>

### disk 저장 방식
- RDB(Snapshotting) 방식 - 순간적으로 메모리에 있는 내용을 disk에 전체를 옮겨 담는 방식

- AOF (Append On File) 방식 - Redis의 모든 write/update 연산 자체를 모두 log 파일에 기록하는 형태
<br>
<hr>
<br>

**Reference**<br>

[Hjjju.log : Redis란 무엇일까? - Redis의 특징과 사용 시 주의점](https://velog.io/@wnguswn7/Redis%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C-Redis%EC%9D%98-%ED%8A%B9%EC%A7%95%EA%B3%BC-%EC%82%AC%EC%9A%A9-%EC%8B%9C-%EC%A3%BC%EC%9D%98%EC%A0%90)<br>
[Jan92 : Redis란? 레디스의 기본적인 개념 (인메모리 데이터 구조 저장소)](https://wildeveloperetrain.tistory.com/21)<br>
[Gyun's 개발일지 : [DB] Redis란 무엇일까? 간단하게 알아보기!](https://devlog-wjdrbs96.tistory.com/374)
