# HttpMessageConverter
![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/2e72e25c-d51e-406a-8421-3294575b80c4)
- 일반적으로 Spring과 템플릿 엔진을 같이 쓰는 경우 controller요청에 따라 ViewResolver가 View를 찾아준다.

- 그러나 JSP, Thymeleaf등과 같은 템플릿 엔진을 제외하고 Vue, React 등을 쓰는 경우 JSON Data만 클라이언트쪽으로 던져준다.

- 이 때 서버에서는 View를 찾는 것이 아니기 때문에 ViewResolver 대신에 HttpMessageConverter 라는게 작동한다.
  - 💡 `@ResponseBody`가 쓰이는 경우에 해당
<br>

- 즉, Spring과 SPA를 같이 사용하는 경우 JSON 데이터를 보내야하기 때문에 `@ResponseBody`를 반드시 써야하고<br>
이 때문에 ViewResolver 대신 HttpMessageConverter가 작동한다.
<br>
<hr>
<br>

### ✔ SpringBoot 동작 과정
![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/8fcf235c-1b7f-4388-a2e3-5a555238359e)
<br>

- String과 같은 기본 문자처리는 StringHttpMessageConverter를 이용

- 객체의 경우에는 MappingJackson2HttpMessageConverter를 이용

- @ResponseBody의 경우에 위의 컨버터를 사용한다고 말했지만 정확히는 다음과 같을 때 사용된다.
  - HTTP 요청 : @RequestBody, HttpEntity(ResponseEntity) (ResponseEntity는 HttpEntity를 상속받았음)
  - HTTP 응답 : @ResponseBody, HttpEntity(ResponseEntity)
<br>
<hr>
<br>

### ✔ HttpMessageConverter 동작 방식
- 코드 확인
  - 주석은 모두 제거
<br>

```java
public interface HttpMessageConverter<T> {

    boolean canRead(Class<?> clazz, @Nullable MediaType mediaType);

    boolean canWrite(Class<?> clazz, @Nullable MediaType mediaType);

    List<MediaType> getSupportedMediaTypes();

    default List<MediaType> getSupportedMediaTypes(Class<?> clazz) {
        return (canRead(clazz, null) || canWrite(clazz, null) ?
                getSupportedMediaTypes() : Collections.emptyList());
    }

    T read(Class<? extends T> clazz, HttpInputMessage inputMessage)
            throws IOException, HttpMessageNotReadableException;

    void write(T t, @Nullable MediaType contentType, HttpOutputMessage outputMessage)
            throws IOException, HttpMessageNotWritableException;

}
```
<br>

- canRead, canWrite를 통해 읽고 쓸 수 있느지에 대한 여부를 확인
  - 컨버터는 요청과 응답 두 경우 모두 사용되기 때문
<br>

- canRead, canWrite 메서드의 인수 타입을 보면 클래스와 미디어타입을 체크
  - 대표적인 컨버터 종류
  - ByteArrayHttpMessageConverter : 클래스 타입 - byte[], 미디어 타입 - */*
  - StringHttpMessageConverter : 클래스 타입 - String, 미디어 타입은 */*
  - MappingJackson2HttpMessageConverter :클래스 타입 - 객체 또는 HashMap, 미디어 타입 application/json
<br>

- 아래는 클래스 및 미디어 타입에 대한 예시
<br>

```java
content-type : application/json

@RequestMapping
void hello(@RequestBody String data) {}
```
> 인수가 String 타입이므로 StringHttpMessageConverter 적용<br>
그 후에 미디어 타입을 체크하게 되는데 해당 컨버터는 모든 미디어 타입을 받아들이니 이것 또한 통과되어 StringHttpMessageConverter를 사용
<br>

```java
content-type : application/json

@RequestMapping
void hello(@RequestBody BepozObject data) {}
```
> 객체타입이니 MappingJackson2HttpMessageConverter 적용<br>
미디어 타입 또한 일치하므로 해당 컨버터를 사용
<br>

- 그렇다면 아래 코드는 어떻게 될 것인가?

```java
content-type : text/html

@RequestMapping
void hello(@RequestBody BepozObject data) {}
```
> MappingJackson2HttpMessageConverter가 적합하다고 판단하겠지만, 미디어 타입 체크를 하는 과정에서 탈락<br>
객체 타입의 Converter는 미디어 타입을 application/json으로 받기 때문<br>
만약 올바른 컨버터를 찾지 못한다면 이에 대한 예외가 발생
<br>
<hr>
<br>

### ✔ HttpMessageConverter 종류
- `org.springframework:spring-web` 패키지에서 http > converter를 확인 가능

- 이 때문에 여러 타입들을 전부 확인하여 변환하는 작업을 거친다.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/3f9de70c-6e7e-4ac4-ad55-53116fd9abfd)
<br>
<hr>
<br>

### ✔ 적용 시점
![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/1814edea-4496-461e-8ce8-9b1f3c886e92)
<br>

- `DispatcherServlet`은 위와 같이 동작한다. (인터셉터와 예외처리 제외한 그림)

- `@RequestMapping` 어노테이션이 핸들러 매핑과 핸들러 어댑터에 대한 정보를 주게되는데, 컨버터는 이 핸들러 어댑터를 수행하는 과정에서 이루어진다.

- 즉, controller는 변환 작업을 마친 데이터를 확인하게 되는 것이다.
<br>
<hr>
<br>

### ✔ RequestMapping HandlerAdapter 동작 방식
![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/98b4911f-c149-4287-9081-238aa7efc040)
<br>

- DispatcherServlet에 의해 핸들러가 맵핑되고, 핸들러 어댑터에서 이 핸들러를 실행할 때, `ArgumentResolver`를 이용해서 파라미터를 넘긴다.

- controller에서 메서드 작성할 시, 사용하던 `@RequestBody`, `Model`, `@ModelAttribute` 등이 모두 `ArgumentResolver`라는 것을 통해서 생성한 후에 해당 파라미터를 넘겨주는 것

- 이 또한 많은 ArgumentResolver가 있는데, `org.springframework:spring-web` 패키지에서 web > method > annotaion에서 확인 가능

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/470df8b7-6429-49d2-b4da-b049cba0031d)
<br>
<hr>
<br>

**Reference**<br>

[인프런 김영한 : 스프링 MVC 1편 - 백엔드 웹 개발 핵심 기술](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-1)<br>
[Bepoz : [Spring] HttpMessageConverter가 적용되는 시점에 대해](https://bepoz-study-diary.tistory.com/374)
