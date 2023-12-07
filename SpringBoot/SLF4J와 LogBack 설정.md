## SLF4J
- SLF4J(Simple Logging Facade for Java)는 다양한 로깅 프레임 워크에 대한 추상화(인터페이스) 역할을 하는 라이브러리이다.

- 추상 프레임워크이기 때문에 단독으로 사용하지 못하고 구현체가 있어야 한다.
  - ex) logback, log4j2
<br>
<hr>
<br>

### ✔ SLF4J 동작과정
- 개발할 때, SLF4J API를 사용하여 로깅 코드를 작성

- 배포할 때, 바인딩된 Logging Framework가 실제 로깅 코드를 수행 

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/4d103645-d9d6-4cf0-93e2-425ff8820a68)

- 이러한 과정은 그림에서 확인할 수 있듯이 SLF4J에서 제공하는 3가지 모듈을 통해 수행될 수 있다. 

- 각각에 대해 알아보자.
<br>

**1. SLF4J Bridging Modules**
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/12d71ec4-9ed8-49c6-a2d0-38e5e71cd1be)

- (SLF4J 이외의) 다른 로깅 API로의 Logger 호출을 SLF4J 인터페이스로 연결(redirect)하여<br>
SLF4J API가 대신 처리할 수 있도록 하는 일종의 어댑터 역할을 하는 라이브러리이다.
<br>
<br>

**2. SLF4J API(인터페이스)**
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/324bccf9-5e1b-4e23-9c34-ecd8543c9802)

- 로깅 동작에 대한 역할을 수행할 추상 메서드를 제공한다.

- 앞서 말했듯이 추상 클래스이기 때문에 이 라이브러리만 단독적으로 쓰일 수 없다.

- 사용시 주의점은 반드시 하나에 API에 하나의 Binding을 둬야한다.
<br>
<br>

**3. SLF4J Binding(.jar)**
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/8d1d2b76-0db6-4807-a0d0-8007b3dadbed)

- SLF4J 인터페이스를 로깅 구현체(Logging Framework)와 연결하는 어댑터 역할을 하는 라이브러리이다.

- SLF4J API를 구현한 클래스에서 Binding으로 연결될 Logger의 API를 호출한다.

- 앞서 API의 주의점과 같이 하나에 API에 하나의 Binding을 둬야한다.
<br>
<hr>
<br>

### ✔ Logback 구현
- Springboot의 로깅 라이브러리는 기본적으로 SLF4J, Logback을 채택하고 있다.

- logback의 Logger 클래스는 SLF4J의 API 스펙을 구현하기 때문에 SLF4J의 API를 그대로 사용할 수 있다.
<br>
<br>

**1. Spring web 추가**
- SpringBoot 프로젝트 생성시 Spring Web을 기본적으로 넣어야 하는데, 이것에 Logback이 포함되어 있다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/ee174edd-795d-4535-8743-4bdc75817552)
<br>
<br>

**2.Logger 클래스 사용**
- 소스 적용 후 index 경로에 접속하여 console 창을 확인해보자.

```java
package com.example.demo;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController

public class TestController {
		
	@RequestMapping(value="/index")
	public String index() {
		Logger logger = LoggerFactory.getLogger(this.getClass());
		logger.info("hello");
		logger.info("This is logger");
		logger.trace("이것은 trace");
		
		return "hello";
	}
	
}

```
<br>
<br>

- 내가 입력했던 로거가 그대로 나오고 있다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/853c33bd-9fff-42e9-8979-0d013b84aaba)
<br>
<br>

- 아무 설정도 하지 않았지만 Logger 클래스를 사용할 수 있었다.

- 이유는 앞에서 말했지만 Dependency로 주입했던 Spring Web starter 패키지를 통해 이미 logback 관련<br>
라이브러리가 이미 추가되었기 때문이다.

- 하지만 이렇게 사용하는 것은 세부 설정이 불가능하기 때문에 개인 프로젝트가 아닌 이상 `xml`파일을 통해<br>
관리가 필요하다.
<br>
<hr>
<br>

### ✔ logback.xml 생성
- 아래와 같은 경로에 `xml`파일을 생성 후 소스를 적용한다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/a4099696-b1bf-4348-a977-5d5e47b878a3)
<br>
<br>

```
<?xml version="1.0" encoding="UTF-8"?>
<configuration>

	<!-- 로그파일 저장 경로 -->
	<property name="LOG_DIR" value="/Users/yogo/logback" />

	<!-- CONSOLE -->
	<appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
		<layout class="ch.qos.logback.classic.PatternLayout">
			<Pattern>
				[%-5level] %d{yyyy-MM-dd HH:mm:ss.SSS} : %30logger{5} - %msg%n
			</Pattern>
		</layout>
	</appender>
	<!-- // CONSOLE -->
	<!-- SYSLOG -->
	<appender name="SYSLOG" class="ch.qos.logback.core.rolling.RollingFileAppender">
		<file>${LOG_DIR}/syslog/syslog.log</file>
		<layout class="ch.qos.logback.classic.PatternLayout">
			<Pattern>
				[%-5level] %d{yyyy-MM-dd HH:mm:ss.SSS} : %30logger{5} - %msg%n
			</Pattern>
		</layout>
		<rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
			<fileNamePattern>${LOG_DIR}/syslog/syslog.%d{yyyy-MM-dd}.%i.log</fileNamePattern>
			<timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
				<maxFileSize>10MB</maxFileSize>
			</timeBasedFileNamingAndTriggeringPolicy>
		</rollingPolicy>
	</appender>
	<!-- // SYSLOG -->
	<!-- ACCESSLOG -->
	<appender name="ACCESSLOG" class="ch.qos.logback.core.rolling.RollingFileAppender">
		<file>${LOG_DIR}/accesslog/accesslog.log</file>
		<layout class="ch.qos.logback.classic.PatternLayout">
			<Pattern>
				%msg%n
			</Pattern>
		</layout>
		<rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
			<fileNamePattern>${LOG_DIR}/accesslog/accesslog.%d{yyyy-MM-dd}.%i.log</fileNamePattern>
			<timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
				<maxFileSize>10MB</maxFileSize>
			</timeBasedFileNamingAndTriggeringPolicy>
		</rollingPolicy>
	</appender>
	<!-- // ACCESSLOG -->
	
	<root level="info">
		<appender-ref ref="CONSOLE" />
		<appender-ref ref="SYSLOG" />
	</root>
	<logger name="com.example.demo" level="debug" additivity="false">
		<appender-ref ref="CONSOLE" />
	</logger>
	<logger name="access-log" level="info" additivity="false">
		<appender-ref ref="ACCESSLOG" />
	</logger>
	
	
</configuration>
```
<br>
<br>

- LOG_DIR property: 로그 파일이 저장될 base 경로

- 사용한 appender class
  - ch.qos.logback.core.ConsoleAppender : console에 로그를 찍기위해 사용
  - ch.qos.logback.core.rolling.RollingFileAppender : 여러개의 파일을 순회하면서 로그를 찍기 위하여 사용
<br>

- appender 태그와 logger 태그 차이
  - appender는 log의 형태를 설정한다고 보면 되고
  - logger는 appender를 통해 적용할 package와 log level을 설정하는 부분이라 볼 수 있다.
<br>

```
log 저장 폴더 만들기
위에서 LOG_DIR으로 /Users/yogo/logback를 지정했다.
그렇기 때문에 해당 경로가 실제 존재하도록 /Users/yogo 경로 하위에 logback이라는 폴더를 만들어줬다.
```
<br>
<hr>
<br>

### ✔ application.properties 수정
- 아래와 같은 코드를 넣어 생성한 `xml`파일을 읽을 수 있게 한다.

- 기본적으로 classpath는 src/main/resources까지 잡혀있기 때문에 그 하위부터 경로를 추가 기재해주면 된다.

```
logging.config=classpath:logback/logback-local.xml
```
<br>
<hr>
<br>

### ✔ 실행
- 서버 실행 후 log가 내가 지정한 경로에 남았는지 확인해보자.

- syslog 라는 폴더에 syslog.log 파일이 생겨있을 것이다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/648869fe-24c2-416f-a53b-249e8b8c5944)
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/807bde7b-9694-48c3-a3c8-a3980f9da897)


**Reference**<br>

[경험의 연장선 : [Logging] SLF4J란?](https://livenow14.tistory.com/63)<br>
[dev_kwonnee.log : Springboot LogBack 설정하기](https://velog.io/@cho876/Springboot-LogBack-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0)

