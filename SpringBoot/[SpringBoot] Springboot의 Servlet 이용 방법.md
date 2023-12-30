## Spring Servlet
- Spring에서는 web.xml을 통해 servlet에 대한 설정을 적는다.

- web.xml에서 초기화 진행시 적용되는 servlet 정보는 servlet-context.xml 에서 적는다.
<br>

### ✔ web.xml
```
// web.xml

<servlet>
    <servlet-name>appServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>/WEB-INF/spring/appServlet/servlet-context.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
</servlet>

<servlet-mapping>
    <servlet-name>appServlet</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>
```
<br>

**Servlet**
- <servlet-name> : 서블릿의 이름을 지정, 여기서는 "appServlet"이라는 이름을 사용한다.

- <servlet-class> : 서블릿 클래스의 경로를 지정, 이 경우에는 스프링의 DispatcherServlet 클래스가 사용된다.

- <init-param>: 서블릿 초기화 매개변수를 설정하는 데 사용
  - <param-name>: 초기화 매개변수의 이름을 지정, 여기서는 "contextConfigLocation"이라는 이름을 사용한다.
  - <param-value>: 초기화 매개변수의 값으로서 스프링 컨텍스트 파일의 위치를 지정한다.<br>
여기서는 "/WEB-INF/spring/appServlet/servlet-context.xml" 경로를 사용합니다.

- <load-on-startup>: 서블릿 컨테이너가 웹 애플리케이션을 시작할 때 서블릿을 초기화하도록 지정,<br>
여기서는 1로 설정되어 있으므로 가장 먼저 초기화된다.
<br>
<br>

**Servlet Mapping**
- <servlet-name>: 매핑할 서블릿의 이름을 지정, 앞서 정의한 "appServlet"을 여기서 사용한다.

- <url-pattern>: 서블릿을 어떤 URL 패턴에 매핑할지 지정, 이 경우에는 "/"로 설정되어 있어서<br>
모든 URL에 대해 이 서블릿이 처리된다. 
<br>
<hr>
<br>

### ✔ servlet-context.xml
- 스프링 MVC에서 사용되는 `DispatcherServlet`이 참조하는 설정파일이다. 

- <annotation-driven /> : 애노테이션 기반의 MVC를 활성화시키는 역할을 한다.
  - @Controller, @RequestMapping 등의 애노테이션을 사용하여 컨트롤러를 정의할 수 있도록 해준다.
<br>

- <resources mapping="/resources/**" location="/resources/" /> : 정적 자원에 대한 매핑을 설정합니다. 
  - Ex) `/resources/css/style.css`와 같은 요청이 오면 실제 파일은 `/resources/css/style.css` 경로에서 찾아서 제공됩니다.
<br>

- <beans:bean> : 뷰 리졸버를 설정하는데 사용된다.
  - 이 설정은 컨트롤러가 반환하는 뷰 이름을 기반으로 실제 JSP 파일의 경로를 결정한다.
<br>

- InternalResourceViewResolver : JSP 뷰를 내부 리소스로 해석합니다.

- <beans:property name="prefix" value="/WEB-INF/views/" />: JSP 파일의 경로 앞에 추가되는 접두어를 설정

- <beans:property name="suffix" value=".jsp" />: JSP 파일의 확장자를 설정한다.

- <context:component-scan base-package="kr.board.controller" /> :
  - 지정된 패키지(kr.board.controller) 밑에서 @Controller 어노테이션이 붙은 클래스들을 찾아 스프링 컨테이너의 빈으로 등록
  - 이렇게 등록된 빈은 스프링 MVC에서 컨트롤러로 활용된다.
<br>
<hr>
<br>

### ✔ 정리
- `web.xml`과 `servlet-context.xml`은 스프링 MVC 애플리케이션에서 필요한 핵심적인 구성 요소들을 정의한다.

- 애노테이션 기반의 MVC를 활성화

- 정적 자원에 대한 핸들링

- 뷰 리졸버 설정

- 컨트롤러의 스캔을 통한 빈 등록
<br>
<hr>
<br>

## SpringBoot Servlet
- 스프링부트에서는 servlet 설정을 자동으로 잡아주기 때문에 별도의 설정이 필요없다.

- 그러나 `ServletComponentScan` 어노테이션을 활용하면 servlet의 직접 등록이 가능하다.
<br>
<hr>
<br>

### ✔ ServletComponentScan
- 메인 메서드에 해당 어노테이션을 추가한다.

- 현재 repo에 등록되어있는 `bootAndvue` 프로젝트에 적용하였다.
<br>
<br>

**ServletComponentScan 설정**
```
// BootAndvueApplication.java

package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.builder.SpringApplicationBuilder;
import org.springframework.boot.web.servlet.ServletComponentScan;
import org.springframework.boot.web.servlet.support.SpringBootServletInitializer;

@ServletComponentScan
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

**서블릿 등록**
- .java 파일을 생성하여 servlet을 만든다.

```
package com.example.demo;

import java.io.IOException;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet(name = "helloServlet", urlPatterns = "/hello")
public class HelloServlet extends HttpServlet {

    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("HelloServlet.service");
        System.out.println("request = " + req);
        System.out.println("response = " + resp);
 
        String username = req.getParameter("username");
        System.out.println("username = " + username);
 
        resp.setContentType("text/plain");
        resp.setCharacterEncoding("utf-8");
        resp.getWriter().write("hello " + username);
    }
}

```
<br>
<br>

**실행 결과**
- 스프링부트 실행 후 `HelloServlet.java`에서 설정한 url로 요청한다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/dff2e254-fc79-4fb2-a2e4-22d2f6c352ec)
<br>
<br>

- 서버 콘솔창
```
HelloServlet.service
request = org.apache.catalina.connector.RequestFacade@3c693032
response = org.apache.catalina.connector.ResponseFacade@5d2901da
username = world
```
<br>
<hr>
<br>

## Spirng MVC 동작 과정
- 앞서 Spring Servlet 설정을 하면 DispatcharServlet이나 view Resolver와 같은 단어가 등장하는데,
MVC 패턴을 이용하기 위해서는 동작 과정을 알아야 한다.

### ✔ Dispatcher-Servlet(디스패처 서블릿)의 동작 방식
- 디스패처 서블릿은 가장 앞단에서 요청을 처리한다고 하여 front-controller라고도 불린다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/104539d6-6f50-40a7-946a-364854e3c333)

1. 클라이언트의 요청을 디스패처 서블릿이 받음
2. 요청 정보를 통해 요청을 위임할 컨트롤러를 찾음
3. 요청을 컨트롤러로 위임할 핸들러 어댑터를 찾아서 전달함
4. 핸들러 어댑터가 컨트롤러로 요청을 위임함
5. 비지니스 로직을 처리함
6. 컨트롤러가 반환값을 반환함
7. 핸들러 어댑터가 반환값을 처리함
8. 서버의 응답을 클라이언트로 반환함
<br>
<hr>
<br>

**Reference**<br>

[망나니개발자 : [Spring] Dispatcher-Servlet(디스패처 서블릿)이란? 디스패처 서블릿의 개념과 동작 과정](https://mangkyu.tistory.com/18)
