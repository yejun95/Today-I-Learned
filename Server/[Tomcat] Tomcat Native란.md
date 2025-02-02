# Tomcat Native란 무엇인가?
- Tomcat Native Library

- Apache 웹 서버의 APR 라이브러리를 Tomcat에서 사용할 수 있게 하는 라이브러리
  - APR (Apache Portable Runtime)의 약자
  - Apache HTTP 2.X를 사용하기 위한 라이브러리
  - Tomcat 설치 시 함께 설치되지 않아 별도의 설치 필요
<br>

- 성능 개선을 위해 Java가 아닌 native로 작성된 connector 모듈을 말한다.

- 즉, APR 기반의 소켓 처리를 이용하고 tomcat에서 HTTP 2.X를 사용하기 위해 쓰는 라이브러리다.
<br>
<hr>
<br>

## ✔ 톰캣 버전별 HTTP Connector
| Connector 유형 | 기본 적용 버전 | 클래스명 | 구현 방식 |
|---------------|--------------|----------------------------------|------------------------------------|
| **BIO Connector** | Tomcat 7 | `org.apache.coyote.http11.Http11Protocol` | Java Blocking API (Pure Java) |
| **NIO Connector** | Tomcat 8 이후 | `org.apache.coyote.http11.Http11NioProtocol` | Java Non-blocking API (Pure Java) |
| **APR Connector** | 별도 설정 필요 | `org.apache.coyote.http11.Http11AprProtocol` | JNI(Java Native Interface) 기반 APR 사용 |
<br>
<hr>
<br>

## ✔️ Tomcat Native를 사용하는 이유
- Apache 웹서버에 APR이라 불리는 성능좋은 네이티브 라이브러리를 Tomcat에서 사용할수 있게 하는 것이 목적

- APR의 소켓 구현 및 난수 생성기를 사용할 수 있다.
<br>
<hr>
<br>

## ✔️ 사용 여부 결정 기준
| 기준                 | NIO (기본)       | APR (Tomcat Native)  |
|----------------------|----------------|---------------------|
| 일반적인 웹 애플리케이션 | ✅ 적합          | ❌ 불필요           |
| 대량의 SSL 트래픽    | ⚠️ JSSE 사용     | ✅ OpenSSL 가속     |
| 대량의 정적 파일 전송 | ⚠️ Java NIO 기반 | ✅ OS sendfile 활용 |
| 대규모 트래픽        | ⚠️ 적절함        | ✅ 네이티브 소켓 최적화 |
| 다양한 OS 지원      | ✅ JVM 기반      | ❌ APR 설치 필요   |
<br>
<hr>
<br>

## ✔️ 결론
- 일반적인 웹 애플리케이션 → 기본 NIO(Non-blocking I/O) 커넥터 사용해도 충분함.

- 대규모 트래픽, 정적 파일 처리, SSL 성능 최적화 필요 → APR 기반 Tomcat Native 사용 고려.

- 운영 환경이 APR을 쉽게 지원하는 경우 → 활용하면 성능 향상 가능.
<br>
<hr>
<br>

**Reference**

[XEST : <아파치-톰캣 기술지원 전문기업 (주)제스트정보기술> Tomcat Native Library란?](https://xestinf.tistory.com/11)<br>
[시골청년의 엔지니어이야기 ( 진반장 ) : tomcat 8.5 tomcat-native 설치 및 설정](https://xinet.kr/?p=1669)<br>
[솔인시스템 : Tomcat Native Library | 톰캣 기술지원 | Tomcat 기술지원 -(주)솔인시스템](https://blog.naver.com/solinsystem/223105390446)
