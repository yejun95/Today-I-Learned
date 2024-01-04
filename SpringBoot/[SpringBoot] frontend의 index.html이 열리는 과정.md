## frontend의 index.html이 열리는 과정
1. 백엔드의 정적 자원으로 사용될 디렉토리가 classpath에 의해 결정된다.
  - spring-boot-autoconfigure / org.springframework.boot.autoconfigure.web / WebProperties.class

```java
public static class Resources {

	private static final String[] CLASSPATH_RESOURCE_LOCATIONS = { "classpath:/META-INF/resources/",
		"classpath:/resources/", "classpath:/static/", "classpath:/public/" };
.....
..........
..................
```

2. 지정된 디렉토리의 리소스가 루트 경로로 서빙된다.

3. context path가 루트 경로인 웹 서버에 브라우저가 접속한다.

4. 브라우저가 웹 서버에 접속 시 index.html을 자동으로 찾는다. 

5. 백엔드에서 서빙된 리소스에서 index.html을 찾고 렌더링 한다.
