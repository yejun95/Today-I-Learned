# JSP란 무엇인가?
- Java Server Pages

- HTML 코드에 JAVA 코드를 넣어 동적 웹페이지(Dynamic Web Page)를 생성하는 웹어플리케이션 도구

- JSP가 실행되면 자바 서블릿(Servlet)으로 변환되며 웹 어플리케이션 서버에서 동작되면서 필요한 기능을 수행하고,
그렇게 생성된 데이터를 웹페이지와 함께 클라이언트로 응답
<br>
<hr>
<br>

## ✔️ 기본구조
- HTML 코드: JSP 페이지의 주요 내용을 구성

- 스크립트릿(Scriptlet): <% %> 사이에 위치하는 Java 코드로, JSP 페이지가 실행될 때 해당 Java 코드도 실행

- 표현식(Expression): <%= %> 사이에 위치하며, 값이 출력될 위치에 사용

- 선언(Declaration): <%! %> 사이에 위치하며, JSP 페이지 내에서 사용될 변수나 메서드를 선언하는 데 사용

- 디렉티브(Directive): JSP 페이지의 동작을 지시하거나 제어하는 태그

- 액션 태그(Action Tags): JSP의 기본 기능을 확장하거나 특정 동작을 수행하기 위해 사용되는 XML 스타일의 태그

- 커스텀 태그(Custom Tags): 개발자가 직접 정의하여 사용하는 태그
<br>
<hr>
<br>

## ✔️ 디렉티브 태그(Directive Tags)
- JSP 페이지의 전체 동작을 제어하는 데 사용

- 주로 세 가지 주요 디렉티브 태그가 있다.

**page 디렉티브**
- JSP 페이지의 응답 및 요청 객체, 페이지 인코딩, 임포트 클래스, 에러 페이지, 세션 사용 여부 등을 지정

```java
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ page import="java.util.Date"%>
<%@ page errorPage="error.jsp"%>
```
<br>

**include 디렉티브**
- 다른 JSP 또는 HTML 파일을 현재 JSP 페이지에 포함시키는 데 사용됩니다. 해당 태그를 만나면 지정된 파일의 내용이 해당 위치에 포함

```java
<%@ include file="header.jsp"%>
```
<br>

**taglib 디렉티브**
- 사용자 정의 태그 라이브러리나 JSTL 등의 태그 라이브러리를 JSP 페이지에 추가하는 데 사용

- 주로 커스텀 태그나 JSTL을 사용할 때 이 디렉티브를 통해 해당 태그 라이브러리를 임포트

```java
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
```
