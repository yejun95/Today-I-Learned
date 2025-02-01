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

**Reference**

[XEST : <아파치-톰캣 기술지원 전문기업 (주)제스트정보기술> Tomcat Native Library란?](https://xestinf.tistory.com/11)<br>

