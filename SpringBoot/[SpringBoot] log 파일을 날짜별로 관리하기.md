## SLF4J와 LogBack을 이용하여 날짜별로 로그 파일 관리하기
- [SLF4J와 LogBack 설정](https://github.com/yejun95/Today-I-Learned/blob/master/SpringBoot/SLF4J%EC%99%80%20LogBack%20%EC%84%A4%EC%A0%95.md)
<br>

**logback.xml**

```java
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
<hr>
<br>

### ✔ 소스 분석
**LOG_DIR Property**
- 로그가 저장될 파일 위치를 설정한다.
<br>
<br>

**CONSOLE Appender : ConsoleAppender**
- 로그가 저장될 형식을 설정한다.

- `[%-5level] %d{yyyy-MM-dd HH:mm:ss.SSS} : %30logger{5} - %msg%n`
  - 로그 레벨 표시, 날짜 형식, 로거 출력 형식, 로그 메시지 출력
  - **[%-5level]** : INFO, WARN 같은 메시지
  - **%d{yyyy-MM-dd HH:mm:ss.SSS}** : Ex) 2023-12-10 20:59:58.330
  - **%30logger{5}** : 로거 이름(logger)을 출력하되, 최소 폭이 30글자가 되도록 오른쪽 정렬 
  만약 로거 이름이 30글자를 초과하면 잘리게 된다.
    - {5}는 로거 이름을 최대 5개의 요소로 제한한다는 의미, 이는 로거 이름을 5개의 단어로 축약하는 역할을 한다.
<br>
<br>

**SYSLOG Appender : RollingFileAppender**
- file : 파일이 저장될 이름 및 경로

- Pattern : 위에서 지정한 양식과 동일

- `<fileNamePattern>${LOG_DIR}/syslog/syslog.%d{yyyy-MM-dd}.%i.log</fileNamePattern>`
  - %i를 붙여서 파일명을 날짜 + index로 지정한다.
  - Ex) 날짜가 2023-01-01이고 인덱스가 1일 때, 파일 경로는 /Users/yogo/logback/syslog/syslog.2023-01-01.1.log가 된다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/706bb15e-796a-4192-ab96-805faec45cda)

- maxFileSize : 로그 파일 1개당 최대 사이즈를 제한
<br>
<br>

**ACCESSLOG Appender : RollingFileAppender**
- 접근 로그

- 웹 서버에 들어오는 각 요청에 대한 정보를 기록한 로그

- 클라이언트가 서버에 어떤 자원에 접근했는지, 언제, 어떤 방식으로, 어떤 응답을 받았는지 등의 정보를 기록한다.
<br>
<br>

**root Logger : info**
- 전역 로그 레벨 설정
<br>
<br>

**logger name="com.example.demo"**
- 특정 패키지에 적용할 Logger 설정
<br>
<br>

**ACCESSLOG Logger**
- ACCESSLOG 앱렌더에만 연결되어 있는 로그 레벨 설정
<br>
<hr>
<br>

### ✔ 날짜 로그 부분
- rollingPolicy 태그에 적용한 TimeBasedRollingPolicy와 filename 태그에 그 답이 있다.

```java
<rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
	<fileNamePattern>${LOG_DIR}/syslog/syslog.%d{yyyy-MM-dd}.%i.log</fileNamePattern>
	<timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
		<maxFileSize>10MB</maxFileSize>
	</timeBasedFileNamingAndTriggeringPolicy>
</rollingPolicy>
```

- `TimeBasedRollingPolicy` : RollingPolicy 중에서 가장 많이 사용하는 롤링 정책으로 일별 또는 월별과 같이 시간을<br>
기준으로 롤오버 정책을 정의한다.

- fileNamePattern : 롤오버 시간은 fileNamePattern의 값에서 유추된다.
  - `<fileNamePattern>${LOG_DIR}/syslog/syslog.%d{yyyy-MM-dd}.%i.log</fileNamePattern>`
  - 해당 코드 에서 `%d{yyyy-MM-dd}.%i.log` 부분이 롤오버 정책의 기준을 정의한 부분이다.
<br>
<hr>
<br>

**Reference**<br>

[사는 이야기 :  3.SpringBoot에서 Logback 사용하기 - Appender와 Policy](https://lovethefeel.tistory.com/89)
