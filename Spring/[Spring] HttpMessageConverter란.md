## HttpMessageConverter
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
<hr>
<br>

**Reference**<br>

[Bepoz : [Spring] HttpMessageConverter가 적용되는 시점에 대해](https://bepoz-study-diary.tistory.com/374)
