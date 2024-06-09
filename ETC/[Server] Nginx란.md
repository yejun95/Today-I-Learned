## Nginx란 무엇인가?

- 트래픽이 많은 웹사이트의 서버(WAS)를 도와주는 비동기 이벤트 기반구조의 웹 서버 프로그램이다.

- 경량 웹서버로써 클라이언트와 WAS 사이에서 정적 파일을 응답해주거나
Reverse Proxy Server로 활용하여 WAS의 부하를 줄일 수 있는 로드 밸런서로 활용된다.
![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/ba02e0d4-5ff9-49fd-8db3-6ae99ee30a7b)
<br>

> Web Server : 단순히 정적 파일을 응답

> WAS(Web Application Server) : 클라이언트 요청에 대해 동적 처리가 이뤄진 후 응답

> Reverse Proxy Server : 웹 서버 앞단에서 클라이언트의 요청에 의한 과부하 방지를 위하여 로드 밸런싱에 사용<br>
[Proxy란?](https://github.com/yejun95/Today-I-Learn/blob/master/ETC/Proxy.md)
<br>

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/f7aaf6a1-1410-439a-9faf-0f9f03e340ac)
<br>

**💡 등장 배경**
- 아파치(Apache)서버의 한계점 등장
  - PREFORK 방식을 통해 미리 프로세스를 만들어 요청이 들어오면 할당을 시킨다. (모두 할당 시 추가 생성)
  - 서버 트랙픽 증가 시 더 이상 과부하로 인한 커넥션을 진행하지 못하였음 (C10K 문제 : Connection 10000 Problem)
  - 커넥션마다 프로세스를 할당하기에는 메모리 부족
  - PREFORK의 장점이였던 확장성이 프로세스의 리소스 양을 늘려버리면서 무거운 프로그램이 되어버림
<br>

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/754b6ee1-2abb-42c0-9f0f-987c9be1f1ba)
<br>

**💡 사용 과정**
- Nginx + Apache
  - 수많은 동시 커넥션을 Nginx가 유지하고, 웹서버이기 때문에 정적 파일은 Nginx가 처리
  - 동적 파일의 경우만 Apache 서버와 커넥션을 형성
<br>

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/db2a88c8-86e5-4d43-9d45-fbecb2c2c95b)
<br>

- 이벤트 구조 : 큐와 스레드 풀을 이용하여 비동기 방식으로 서버 자원을 효율적으로 사용
![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/40855e8e-b18e-4884-9bca-fc2b6dcdfee7)
<br>

**💡 장단점**
- 장점 : 프로세스의 효율적 사용하기 때문에 경량 웹서버가 가능

- 단점 : 개발자가 직접 모듈을 만들기가 까다롭다.
<br>

**✔ 정리**
- Nginx의 기능
  - 웹 서버
  - 로드 밸런서
  - 캐싱
  - CORS처리
  - TCP/UDP 커넥션 부하 분산
 
- 기본적으로 정적 처리 웹서버가 맞지만 CGI 또는 프록시 기능 사용 시 동적 요청 처리 가능
<br>

**Reference**<br>

[ssdragon - Nginx란](https://ssdragon.tistory.com/60)<br>
[losskatsu - proxy 서버란](https://losskatsu.github.io/it-infra/reverse-proxy/#2-%ED%8F%AC%EC%9B%8C%EB%93%9C-%ED%94%84%EB%A1%9D%EC%8B%9Cforward-%EC%84%9C%EB%B2%84%EB%9E%80)




