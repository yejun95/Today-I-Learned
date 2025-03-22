## application.properties 란 무엇인가?
-  Spring Boot 애플리케이션의 구성 속성을 설정하는 데 사용되는 파일이다.

-  이 파일은 애플리케이션의 클래스 경로에 위치하며, 키-값 쌍의 형식으로 구성된다.
  - 해당 파일을 사용하면 애플리케이션의 동작을 구성할 수 있다.
  - 클래스 경로(class path) : 컴파일 하거나 프로그램을 실행할 때 읽어올 클래스의 위치를 나열해 놓은 것

- properties 예제 
```java
// key-value 쌍의 형식

## 서버 포트
server.port=80


## 임의 경로 변수
ffmpeg.location=C:/ffmpeg/bin/ffmpeg.exe
ffprobe.location=C:/ffmpeg/bin/ffprobe.exe

## 임의 문자 변수
ffmpeg.text.hh=hi

```
<br>

- 예를 들어, 데이터베이스 연결 정보, 로깅 설정, 서버 포트, 보안 구성 등을 지정할 수 있다.
<br>
<hr>
<br>

### ✔ 특징
- 개발환경, 운영환경, 실제 서비스 등 다양한 환경에 따른 설정 옵션을 적용가능하다.
  - 필요한 환경에 따라, 그때 그때 세팅을 다르게 하여 적용이 가능하다는 뜻이다.
  - 다양한 환경에 배포될 때마다 환경에 따른 설정 값을 프로퍼티 파일을 통해 코드 변경없이 바꿀 수 있다. 
<br>

- 밑에 사진과 같이 `.properties` 파일을 여러개 만들어서 여러 환경 세팅이 가능하다.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/d6026750-6302-4e50-91da-9716e6618d0e)
<br>
<br>

- 가장 많이 접하게 되는 예시로는 로컬에서 테스트하는 database 접속 정보와 실서버에 이용하는 database 접속 정보가 다를 때이다.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/2a346494-a7ca-4744-9500-eba482bde7b4)



<br>
<hr>
<br>

**Reference**<br>

[olrlobt : Spring boot @Value 어노테이션으로 application.properties의 값 가져오기](https://olrlobt.tistory.com/52)<br>
[Contributor9 : Java 개발 환경에 따라 각각 환경 파일 구성 방법: application.properties](https://adjh54.tistory.com/200)<br>
[Jan92 : springboot 개발 환경에 따른 properties 사용 방법](https://wildeveloperetrain.tistory.com/8)
