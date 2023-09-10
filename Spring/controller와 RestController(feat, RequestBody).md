## controller와 RestController의 차이
- Spring에서 컨트롤러를 지정해주기 위한 어노테이션은 @Controller와 @RestController가 있다.

- 전통적인 Spring MVC의 컨트롤러인 @Controller와 Restuful 웹서비스의 컨트롤러인 @RestController의 주요한 차이점은<br>
HTTP Response Body가 생성되는 방식이다.
<br>
<hr>
<br>

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

- 예제 소스 (ajax통신을 통한 데이터 request, response)

- controller : 데이터 응답을 Json형태로 반환 `@ResponseBody`

- main.jsp : 응답받는 데이터를 Json으로 받는다. `dataType : 'Json'`
```javascript
// controller

@Controller
public class BoardController {

	@Autowired
	BoardMapper boardMapper;

	@RequestMapping("/selectAll")
	public List<Board> boardList(){
		List<Board> list = boardMapper.getLists();
		return list;
	}
	
}

////////////////////////////////////////////////////

// main.jsp

function loadList() {
		$.ajax({
			url : 'board/selectAll',
			type : 'get',
			dataType : 'json',
			success : makeView, // 콜백함수
			error : function() {alert("error");}
		});
	};
```
<br>
<hr>
<br>

### ✔ RestController
- 기존 controller에 `ResponseBody`를 추가한 것으로 Json 형태로 객체를 `request`받고 `response`한다.

- 결합된 것이기 때문에 `@Response` 어노테이션을 기입하지 않아도 **자동으로 적용**되어 있다.
<br>

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/14912c8e-d9c3-4b06-9e82-04bebe572aa4)
<br>

- 예제 소스 (ajax통신을 통한 데이터 request, response)

- RestController : `@RestController`라는 어노테이션이 붙고 `@RequestMapping` 대신에 해당하는 타입 어노테이션이 붙는다.

- main.jsp : 응답받는 데이터를 Json으로 받는다. `dataType : 'Json'`
```javascript
// RestController

@RestController
@RequestMapping("/board")
public class BoardRestController {

	@Autowired
	BoardMapper boardMapper;
	
	@GetMapping("/selectAll")
	public List<Board> boardList(){
		List<Board> list = boardMapper.getLists();
		return list;
	}
}

////////////////////////////////////////////

// main.jsp

function loadList() {
		$.ajax({
			url : 'board/selectAll',
			type : 'get',
			dataType : 'json',
			success : makeView, // 콜백함수
			error : function() {alert("error");}
		});
	};

```
<br>
<hr>
<br>

### ✔ 핵심 포인트
<br>

**RestController**
- Json형태로만 데이터를 request, response 할 수 있다.

- Ajax를 통한 body로 data를 보내주어야 할 때 Json 형태로만 보내야 한다.
  - `JSON.Stringify` 사용

- RestController에서는 `@RequestBody` 어노테이션을 통해 넘어온 Json 데이터를 받아야한다.
  - `@RequestBody`는 자바 Vo 객체(**참조타입**)만 인식
  - 이말은 즉, 상세보기나 삭제할 때 `int idx` 형식으로 인자값을 받게 되면 `JSON.strinfigy`로 data를 전송해도 controller에서 인식불가<br>
    int, String의 경우 **원시타입**이기 때문이다.
  - 그렇기에 url에 쿼리파라미터를 붙여서 보내게 되고, 매개변수에 @PathVariable을 사용<br>
[원시타입과 참조타입이란?](https://github.com/yejun95/Today-I-Learn/blob/master/Javascript/%EC%9B%90%EC%8B%9C%ED%83%80%EC%9E%85%EA%B3%BC%20%EC%B0%B8%EC%A1%B0%ED%83%80%EC%9E%85.md)<br>
[JSON,parse와 JSON.stringify의 차이](https://github.com/yejun95/Today-I-Learn/blob/master/Javascript/JSON.parse()%2C%20JSON.stringify().md)
<br>

**예제 소스 (update, delete ajax 통신을 통한 controller와 RestController 비교)**

**controller**<br>
```javascript
// controller

@Controller
public class BoardController {

	@Autowired
	BoardMapper boardMapper;
	
	@RequestMapping("/update")
	public @ResponseBody void update(Board vo) {
		boardMapper.boardUpdate(vo);
	};
	
	@RequestMapping("/delete")
	public void delete(@RequesParam("idx") int idx) {
		boardMapper.boardDelete(idx);
	};
}


///////////////////////////////////////////////////////////

// main.jsp

// 게시글 수정
	function goUpdate(idx) {
		$.ajax({
			url : "/update",
			type : "post",
			data : {"idx" : idx, "title" : title, "content" : content},
			success : loadList,
			error : function() { alert("err"); }
		});
	};

// 게시글 삭제
	function goDelete(idx) {
		$.ajax({
			url: "/delete,
 			type: "post",
                        data: {"idx" : idx},
			success: loadList,
			error: function(){ alert("err");}
		});
	};
```
<br>

- controller
  - 수정 : post요청으로 들어온 data를 자바 객체로 받고 `@ResponseBody`로 응답한다.;
  - 삭제 : 요청이 들어온 `idx`를 `@RequesParam`으로 받아서 응답한다.
 
- main.jsp : 특별한 것 없이 필요한 파라미터들을 data에 담아 보낸다.
<br>

**RestController**<br>
```javascript
// RestController

@RestController
@RequestMapping("/board")
public class BoardRestController {

	@PutMapping("/update")
		public void update(@RequestBody Board vo) {
			boardMapper.boardUpdate(vo);
		};

	@DeleteMapping("/delete/{idx}")
	public void delete(@PathVariable("idx") int idx) {
		boardMapper.boardDelete(idx);
	};
}

////////////////////////////////////////////////////////

// main.jsp

// 게시글 수정
	function goUpdate(idx) {		
		$.ajax({
			url : "board/update",
			type : "put",
			data : JSON.stringify({"idx" : idx, "title" : title, "content" : content}),
			contentType: "application/json;charset=utf8",
			success : loadList,
			error : function() { alert("err"); }
		});
	};

// 게시글 삭제
	function goDelete(idx) {
		$.ajax({
			url: "board/delete/"+idx,
 			type: "delete",
			success: loadList,
			error: function(){ alert("err");}
		});
	};
```

- RestController
  - 수정 : put요청을 받고있으며, Json으로 넘어온 데이터를 `@RequestBody`를 통해 받는다.
  - 삭제
    - 쿼리파라미터로 들어온 값을 url에 `{idx}`형태로 받는다. 
    - RestController는 Json으로만 요청, 응답하고 `@RequestBody`는 자바 Vo 객체(참조타입)만 인식가능하기 때문에<br>
쿼리 파라미터로 들어온 변수를 `@Pathvariable`을 통해 받는다.

- main.sjp
  - 수정
    - data : RestController가 Json형태로만 데이터를 받기 때문에 `JSON.Stringify`를 통해 객체를 넘긴다.
    - contentType: 내가 보내는 data가 json형태임을 RestController에게 알려준다.
  - 삭제 : 원시타입은 data의 형태로 보낼 수 없기 때문에 쿼리파라미터를 통해 idx값을 보낸다.
<br>
<hr>
<br>

### ✔ 요점 정리
- Json으로 보내냐 받느냐 관점의 차이

- 이로 인한 쿼리파라미터 사용 및 `@PathVariable`, `@RequestBody`등의 추가적인 어노테이션 생성

- RestController가 참조타입만 가능하기 때문에 ajax의 data 속성으로 보낼 수 있느냐 없느냐의 차이
<br>
<hr>
<br>

**Reference**<br>

[망나니개발자 : @Controller와 @RestController 차이](https://mangkyu.tistory.com/49)<br>
[박매일 : 스프링 프레임워크는 내손에 스프1탄. MVC02 파트](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84-spring-1/dashboard)<br>
