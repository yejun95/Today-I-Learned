## SpringBoot에서 회원가입 API에 대한 조건식 적용
- 클라이언트가 회원가입시 API를 서버에 요청하면 서버는 DB에서 데이터를 확인한다.
  - 아이디가 중복이면 Exception 발생
  - 아이디가 신규면 회원가입 진행
<br>

### ✔ controller
```
// 회원가입
@PostMapping("/post/user/signUp")
public String postSignUp(@RequestBody User vo) {
List<User> list = userMapper.getLists();
try {
	if(list.stream().noneMatch(d -> 
		d.getUserSeq().equals(vo.getUserId()) ||
		d.getUserId().equals(vo.getUserId()) ||
		d.getUserToken().equals(vo.getUserToken()))) {
		String token = jwtService.createToken(vo.getUserId(), (2*1000*60));
		vo.setUserToken(token);
		userMapper.postSignUp(vo);
		System.out.println("성공");
	} else {
		throw new Exception();
	}
} catch (Exception e) {
	System.out.println("회원가입 실패");
	e.printStackTrace();
	return "fail";
} 
return "success";
}
```
**회원가입 진행**
- post 요청이 들어오면 회원가입을 진행한다.

- `getLists` 메서드 : DB에서 user 목록을 전부 조회

- user 목록을 전부 조회하여 변수에 담고, 이를 stream을 사용해 데이터를 확인한다.
  - stream : 자바 8부터 지원, 컬렉션에 들어있는 데이터를 손쉽게 처리할 수 있다.
<br>

- noneMatch : stream에서 사용하는 메서드로, 만족하는 조건이 하나라도 있으면 false를 리턴한다.
  - 해당 로직에서는 유저 아이디, seq, token 값을 or 조건으로 확인하여, 값이 1개라도 일치한다면 false를 리턴하는 것이다.
<br>

- token : 정의하였던 토큰 메서드를 사용
  - 토큰 생성후 자바 VO 객체에 값을 바인딩한다.
  - [SpringBoot + JWT 적용](https://github.com/BJSNuruhee/levelup/issues/31)
<br>

- `postSignUp`이라는 회원가입 메서드를 실행한다.

- 성공하면 클라이언트에 `success`라는 문자열을 리턴한다.
<br>
<br>

**noneMatch에 걸려 false가 리턴 될 시**
- if문의 else로 빠지게 되며, Exception을 리턴한다.

- Exception이 리턴되어 catch로 진입하여 Exception을 콘솔에 출력한다.

- 이후 클라이언트에게 `fail`이라는 문자열을 리턴한다.
