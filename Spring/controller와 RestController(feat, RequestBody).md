## controller와 RestController의 차이
- Spring에서 컨트롤러를 지정해주기 위한 어노테이션은 @Controller와 @RestController가 있는데, 전통적인 Spring MVC의 컨트롤러인 @Controller와<br>
Restuful 웹서비스의 컨트롤러인 @RestController의 주요한 차이점은 HTTP Response Body가 생성되는 방식이다.

### ✔ controller
- 전통적인 MVC의 controller의 경우 주로 view를 반환하기 위해 사용하고,<br>
아래와 같은 과정을 통해 Client의 요청으로부터 view를 반환한다.
<br>

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/9d76b6f6-f9e8-4215-86db-3ab4986dfc6c)
<br>

- data를 응답해야 하는 경우 `@ResponseBody' 어노테이션을 통해 리턴한다.
<br>

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/8187a869-b224-44c0-b4cf-6e21cbd3a26e)
<br>

- 예제 코드
```java
// controller

@RequestMapping("/board/selectAll")
	public @ResponseBody List<Board> boardList(){
		List<Board> list = boardMapper.getLists();
		return list;
	}

```

레스트콘트롤러 -> 제이슨으로 request, response

ajax에서 data 보낼 때 JSON.stringify로 보냄

그럼 콘트롤러에서 값 받을 때 RequestBody 사용
    - RequestBody는 자바 VO 객체(참조타입)만 인식 
    - 이말은 즉, 상세보기나 삭제할 때 int idx 형식으로 인자값을 받게 되면 JSON.strinfigy로 data를 전송해도 콘트로러에서 인식불가
       int, String의 경우 원시타입이기 때문
    - 그렇기에 url에 쿼리파라미터를 붙여서 보내게 되고, 매개변수에 @PathVariable을 사용


**Reference**<br>

[망나니개발자 : @Controller와 @RestController 차이](https://mangkyu.tistory.com/49)<br>
