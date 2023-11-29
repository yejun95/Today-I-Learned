## SpringBoot에서 외장 톰캣 설정 방법
### ✔ 1. Tomcat 다운로드
- 먼저 SpringBoot 버전에 맞는 tomcat을 다운로드 받아야한다.

- `pom.xml`에서 SpringBoot 버전 확인
  - 현재 내 버전은 2.7.17 이다.
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/bfc30d37-2a7a-4728-8d14-50338cd74910)

- maven repository에 가서 내 SpringBoot 버전이 어떤 톰캣을 쓰는지 확인한다.
  - 9.0.82 버전으로 확인이 되고, 10.1.16까지 업데이트 되어있다.
  - 해당 범위에 맞는 톰캣을 다운로드 받으면 된다.
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/28a03ef1-7b3e-4195-bb35-929e127cf178)

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/d76e73a4-df93-402f-82be-27a85681c1a8)

- 톰캣 홈페이지에서 다운로드 받는다.
  - https://tomcat.apache.org/download-10.cgi
<br>
<br>

### ✔ 2. pom.xml 에서 packaging, dependency 설정

```
<packaging>war</packaging>

<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-tomcat</artifactId>
  <scope>provided</scope>
</dependency>
```
<br>
<br>

### ✔ 3. SpringBootApplication에서 SpringBootServletInitializer를 상속시켜준다.
- WAR 파일로 패키징된 경우에 외부 서블릿 컨테이너에 애플리케이션을 배포할 때 사용된다.

```java
@SpringBootApplication
public class BootAndvueApplication extends SpringBootServletInitializer{

	@Override
	protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
		return builder.sources(BootAndvueApplication.class);
	}
	
	public static void main(String[] args) {
		SpringApplication.run(BootAndvueApplication.class, args);
	}
}
```
<br>
<br>

### ✔ 4. 톰캣에 SpringBoot 프로젝트 올리기
- 프로젝트 우클릭 -> Properties -> Project Facets 에서 Dynamic Web Module을 체크한다.
  - Dynamic Web Module 설정은 WAR파일을 생성하는 데 필요하다.
  - WAR 파일은 웹 애플리케이션을 배포하는 데 사용되며, 이 파일을 외부 톰캣과 같은
   서블릿 컨테이너에 배포하여 애플리케이션을 실행할 수 있게 한다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/b804eadf-2a7e-4622-9c6d-cf843fc68cc0)
<br>

- 이후 Runtime tab에서 톰캣을 설정한다.
  - 해당 글에서는 미리 다운받아져 있는 톰캣을 미리 등록했다.
 
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/faf5666e-6b5e-48e7-9207-bc54afbf0232)
<br>
<br>

### ✔ 5. 톰캣 실행하기
- 톰캣에 등록했다면 servers 탭에서 톰캣에 등록된 SpringBoot 프로젝트를 볼 수 있다.

- 톰캣을 우클릭하여 start 시키면 정상적으로 SpringBoot 프로젝트가 외장 톰캣으로 실행이 된다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/c5cd1946-ad4d-444a-9620-1dd142686b52)
<br>
<br>

- 외장 톰캣으로 설정된 8080 포트로 실행이 되는 걸 확인할 수 있다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/d1a758e1-d063-4b45-bad6-405cb62a233f)
