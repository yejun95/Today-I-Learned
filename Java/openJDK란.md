## openJDK란 무엇인가?
- open JDK는 오픈소스 기반의 무료로 사용할 수 있는 JDK를 말한다.

- Java SE(Standard Edition)의 무료 및 오픈 소스 구현체로, Java SE에서 정의된 인터페이스를 구현한 구현체이다.

- Java SE와 같은 API, 기능을 제공하며 호환성을 유지한다.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/12562a78-016f-47a3-88a6-8ef9dd7c1df2)
<br>

- Java SE : Java 프로그래밍 언어의 표준 사양
  - Java 애플리케이션을 구축하기 위한 기본 API를 제공

- 그렇다면 JDK는 무엇인가?
<br>
<hr>
<br>

### ✔ JDK란?
- Java Development Kit

- Java 프로그램을 개발하기 위해 필요한 도구 모음이다.

- JDK는 Java 컴파일러, 디버깅 도구, 자바 가상 머신(JVM)을 포함하고 있다.

- JRE : 자바 애플리케이션을 실행하기 위한 환경을 제공하는 소프트웨어
  - 자바 프로그램을 실행하기 위해 필요한 라이브러리, 실행 환경 등을 포함하고 있어,<br>
  자바 프로그램이 운영체제와 하드웨어에 구애받지 않고 동작할 수 있도록 한다.

- JVM : 자바 프로그램을 실행하는데 사용되는 가상 컴퓨터
  - 운영체제와 독립적인 환경에서 실행될 수 있도록 해준다.
![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/4ab6e056-e687-4380-b464-5faa40aeae80)
<br>
<hr>
<br>

### ✔ Open JDK의 종류
- Open JDK의 경우 오라클에서 만든 Java SE의 인터페이스를 기반으로 제공한다.

- 이들의 차이는 JDK의 성능, 안정성, 보안 등에 따라 상이하다.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/42c71efb-3e22-4d65-ba9b-fcd23ef6a765)
<br>
<hr>
<br>

### ✔ 왜 사용하는 것인가?
- Oracle의 JDK가 2018년 7월부로 Java SE Supscription이라는 이름의 년 단위<br>
유료 구독형 라이센스로 새롭게 개편되었다.

- 때문에 기업에서는 비용 부담이 될 수 있기 때문에 Open JDK를 사용한다.

- 그러나 비상업적인 용도에 한해서는 Oracle JDK도 이전과 같이 무료로 사용이 가능하다.
<br>
<hr>
<br>

### ✔ OpenJDK는 운영 환경에 부적합한가?
- 결론부터 보자면 TCK 인증을 받은 OpenJDK 기반의 빌드 버전을 사용하면 운영 환경에 아무런 문제가 없다.

- TCK (Technology Compatibility Kit)
  - SUN의 자바 기술을 구현한 licensee들의 vm이 규격에 맞게 제대로 구현되었는지 검증하는 테스트 프로그램과 그 도구
  - sun의 자바기술을 이용하여 구현한 모든 vm들이 정확하고, 완전하고, 일관성 있게 구현되었는지를 검증
  - 해당 JDK가 JSR표준에 맞게 구현되었는지 테스트할 수 있으며 통과 시 인증을 받을 수 있다.
  - 인증 주관은 Oracle사에서 하고 있다.
  - [JSR 정보 확인](https://www.jcp.org/en/home/index)
 
![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/cc14bc98-aa22-4d16-b574-07b692bfca7a)
<br>
<hr>
<br>

**Reference**<br>

[Contributor9 : JDK(Java Development Kit), Open JDK 이해하기](https://adjh54.tistory.com/213)<br>
[rwd337 : openjdk와 oracle jdk 차이](https://rwd337.tistory.com/191)<br>
[월든의 더 나은 개발하기 : JCP, JSR, TCK란 무엇인가](https://betterdev.tistory.com/3)
