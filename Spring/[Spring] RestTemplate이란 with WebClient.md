# RestTemplate 이란?
- HTTP 통신을 위한 도구로 RESTful API 웹 서비스와의 상호작용을 쉽게 외부 도메인에서 데이터를 가져오거나 전송할 때 사용되는 스프링 프레임워크의 클래스를 의미

![image](https://github.com/user-attachments/assets/fac65d67-d7fb-4f35-ae4a-84cae76460e1)
<br>
  
- Rest방식의 API를 호출할 수 있는 Spring의 내장 클래스

- Spring 3.0부터 지원하는 Spring의 HTTP 통신 템플릿

- JSON, XML, String 응답을 모두 받는다.

- Blocking I/O 기반의 동기방식을 사용한다.
  - Rest API 호출 후 응답을 받을 때 까지 기다림 (like async/await)
<br>

- Header + Content-Type을 설정해서 외부 API 호출이 가능
<br>

```
 [ 더 알아보기 ]

💡 The RestTemplate will be deprecated?

Spring Framework 5.2.1 버전에서 RestTemplate가 Depreacted 되고 Spring Framework 5.x 버전부터 생긴
WebClient로 Migration을 하라는 이야기가 있다.

이런 이야기가 나오기는 하지만 현재 6.0.11 버전에서까지 릴리즈가 되고 있기에 Deprecated 되지 않는것 같다.
```
<br>

![image](https://github.com/user-attachments/assets/f608a90b-a716-45af-8fcd-cbe43809b640)
> https://docs.spring.io/spring-framework/docs/5.2.1.RELEASE/javadoc-api/index.html?org/springframework/web/client/RestTemplate.html
<br>
<hr>
<br>

## ✔️ RestTemplate 메서드
| 메서드           | HTTP    | 설명                                                                 |
|------------------|---------|----------------------------------------------------------------------|
| getForObject     | GET     | 주어진 URL 주소로 HTTP GET 메서드로 객체를 반환받는다               |
| getForEntity     | GET     | 주어진 URL 주소로 HTTP GET 메서드로 결과를 ResponseEntity로 반환받는다 |
| postForLocation  | POST    | POST 요청을 보내고 결과로 헤더에 저장된 URI를 결과로 반환받는다     |
| postForObject    | POST    | POST 요청을 보내고 객체로 결과를 반환받는다                         |
| postForEntity    | POST    | POST 요청을 보내고 결과를 ResponseEntity로 반환받는다               |
| delete           | DELETE  | 주어진 URL 주소로 HTTP DELETE 메서드를 실행한다                     |
| headForHeaders   | HEADER  | 헤더의 모든 정보를 얻을 수 있으며 HTTP HEAD 메서드를 사용한다       |
| put              | PUT     | 주어진 URL 주소로 HTTP PUT 메서드를 실행한다                        |
| patchForObject   | PATCH   | 주어진 URL 주소로 HTTP PATCH 메서드를 실행한다                      |
| optionsForAllow  | OPTIONS | 주어진 URL 주소에서 지원하는 HTTP 메서드를 조회한다                 |
| exchange         | any     | HTTP 헤더를 새로 만들 수 있고 어떤 HTTP 메서드도 사용가능하다       |
| execute          | any     | Request/Response 콜백을 수정할 수 있다 
<br>

- 기본적으로 `exchange` 함수를 많이 사용 (uri, header, HTTP 메서드 등을 자유롭게 넣을 수 있기 때문)

- get 요청에 헤더가 필요할 경우 get Method에서는 header를 추가할 수 없으므로 exchange method에 header를 넣어 사용
<br>
<hr>
<br>

## ✔️ 사용법
```java
// RestTemplate 생성
RestTemplate restTemplate = new RestTemplate();

// 요청 매개변수 설정
HttpHeaders headers = new HttpHeaders();

headers.setContentType(MediaType.APPLICATION_JSON);

HttpEntity<RequestDto> request = new HttpEntity<>(requestDto, headers);

// HTTP 요청 및 응답 처리
ResponseDto responseDto = restTemplate.exchange(url, HttpMethod.POST, request, ResponseDto.class).getBody();
```
<br>
<hr>
<br>

## ✔️ HTTP 요청 및 응답 처리 방법
- RestTemplate을 이용하여 HTTP 요청 및 응답 처리를 수행하는데 여러 가지 형식으로 요청 및 응답 처리를 할 수 있다.

- JSON, XML 및 바이너리 데이터 형식과 같은 데이터 형식으로 응답 처리를 할 수 있고, 또한 HTTP 요청 및 응답의 부분을 추출하거나 수정할 수 있다.

```java
RestTemplate restTemplate = new RestTemplate();

HttpHeaders headers = new HttpHeaders();

headers.add("Content-Type", "application/json");

headers.add("Authorization", "Bearer <access_token>");
```
<br>

| 헤더         | 이름 값              | 설명                                           |
|--------------|----------------------|------------------------------------------------|
| Accept       | -                    | 클라이언트가 서버에서 받기 원하는 미디어 타입을 지정합니다. |
| Accept       | text/plain           | 일반적인 텍스트 형식을 지정합니다.            |
| Accept       | application/json     | JSON 형식을 지정합니다.                        |
| Accept       | application/xml      | XML 형식을 지정합니다.                         |
| Content-Type | -                    | 서버에서 클라이언트로 보내는 미디어 타입을 지정합니다. |
| Content-Type | text/plain           | 일반적인 텍스트 형식을 지정합니다.            |
| Content-Type | application/json     | JSON 형식을 지정합니다.                        |
| Content-Type | application/xml      | XML 형식을 지정합니다.                         |
<br>
<hr>
<br>

## ✔️ HTTP 요청에 대한 요청 헤더 및 쿼리 매개변수 설정
- HTTP 요청에 대한 요청 헤더 및 쿼리 매개변수를 설정할 수 있다.

- 요청 헤더는 RestTemplate의 HttpHeaders 클래스를 사용하여 설정할 수 있다.   

- HttpHeaders 클래스의 add() 메서드를 사용하여 요청 헤더에 새 항목을 추가할 수 있다.

- 쿼리 매개변수는 RestTemplate의 exchange() 메서드를 호출할 때 UriComponentsBuilder를 사용하여 설정할 수 있다.

- UriComponentsBuilder의 queryParam() 메서드를 사용하여 쿼리 매개변수를 추가할 수 있다.
<br>

```java
RestTemplate restTemplate = new RestTemplate();

HttpHeaders headers = new HttpHeaders();
headers.setAccept(Collections.singletonList(MediaType.APPLICATION_JSON));

UriComponentsBuilder builder = UriComponentsBuilder.fromHttpUrl("<http://example.com/api/users>")
        .queryParam("page", 2)
        .queryParam("size", 10);

HttpEntity entity = new HttpEntity<>(headers);

ResponseEntity response = restTemplate.exchange(
        builder.toUriString(),
        HttpMethod.GET,
        entity,
        String.class);
```
<br>
<hr>
<br>

## ✔️ HTTP 요청 및 응답에 대한 로깅 제공
- HTTP 요청 및 응답에 대한 로깅을 제공

- RestTemplate Bean을 구성하는 데 사용되는 HttpClient는 HTTP 요청 및 응답을 로깅할 수 있는 HttpClientInterceptor를 제공

- HttpClientInterceptor의 구현체로 재 구성한다.
<br>

```java
public class CustomInterceptor implements ClientHttpRequestInterceptor {
    @Override
    public ClientHttpResponse intercept(HttpRequest request, byte[] body,
            ClientHttpRequestExecution execution) throws IOException {
        // Request 수정
        request.getHeaders().add("Custom-Header", "Custom-Value");

        // Request 정보 로깅
        log.info("Request URI : {}", request.getURI());
        log.info("Request Method : {}", request.getMethod());
        log.info("Request Headers : {}", request.getHeaders());
        log.info("Request Body : {}", new String(body, "UTF-8"));

        // 다음 인터셉터 또는 요청 실행
        ClientHttpResponse response = execution.execute(request, body);

        // Response 정보 로깅
        log.info("Response Status : {}", response.getStatusCode());
        log.info("Response Headers : {}", response.getHeaders());

        return response;
    }
}
```
<br>

- 해당 CustomInterceptor는 RestTemplate 객체 생성 시에 interceptors 속성에 대한 구현체로 넣을 수 있다.

```java
RestTemplate restTemplate = new RestTemplateBuilder()
        .interceptors(new CustomInterceptor())
        .build();
```
<br>
<hr>
<br>

## ✔️ RestTemplate vs WebClient
- 동기적으로 처리되는 RestTemplate과 비동기적으로 처리되는 WebClient를 비교 진행

| 구분                 | RestTemplate           | WebClient               |
|----------------------|------------------------|--------------------------|
| 요청/응답            | 동기                   | 비동기                   |
| Connection pool      | Apache Http Client     | Netty                    |
| 기본 인증            | 지원                   | 지원                     |
| OAuth2 인증          | 지원                   | 지원                     |
| JSON 처리            | Jackson, Gson 등       | Jackson, Gson 등         |
| XML 처리             | JAXB, Jackson 등       | Jackson 등               |
| multipart/form-data  | 지원                   | 지원                     |
| 쿠키                 | 지원                   | 지원                     |
| 헤더 설정            | 지원                   | 지원                     |
| 요청 시간 초과 설정 | 지원                   | 지원                     |
| SSL 인증서 검증      | 지원                   | 지원                     |
| 로깅                 | 지원                   | 지원                     |
| 성능                 | 느림                   | 빠름                     |
<br>
<hr>
<br>

**Reference**<br>

[쮸니이 : RestTemplate 이란 무엇인가?](https://jjunn93.com/entry/Spring-RestTemplate-%EC%9D%B4%EB%9E%80)<br>
[Contributor9 : Spring Boot Web 활용 : RestTemplate 이해하기](https://adjh54.tistory.com/234)
