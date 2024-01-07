## SpringBoot + Vue3 JWT를 이용한 로그인 구현
- [JWT란 무엇인가?](https://github.com/yejun95/Today-I-Learned/blob/master/ETC/JWT%EB%9E%80%20%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80.md)
<br>

**기본 흐름**
- 회원가입시 ID마다 토큰이 생성되어 부여된다.

- 클라이언트에서 axios를 통해 id, pw를 서버로 보낸다.

- 서버에서 DB를 조회하여 해당 id의 유/무를 파악하여 쿠키를 생성할지 Exception처리할지 결정한다.
  - DB쪽 코드는 생략한다.
<br>

- DB에 아이디가 있다면 클라이언트로 쿠키를 리턴한다.
  - 쿠키에 회원가입때 부여받은 토큰 값을 쿠키의 value로 담는다.
<br>

- 클라이언트로 리턴된 쿠키 id를 store(pinia)에 저장하여 홈페이지 전체에서 로그인을 유지한다.

- 새로고침하면 쿠키가 풀리기 때문에 App.vue에서 쿠키를 지속적으로 체크하는 함수를 삽입한다.
  - 해당 함수 실행 시, 서버에서 쿠키 id를 확인하여 해당 값이 있으면 동일한 데이터를 다시 리턴한다.
  - 때문에 store에 쿠키 값이 지속적으로 유지된다.
<br>
<hr>
<br>

### ✔ frontend 구조 구조
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/57b0fcb0-7d69-450d-bdfa-17ac20767c52)
<br>
<hr>
<br>

### ✔ backend 구조
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/d2d0d141-c50c-42fb-b610-8b87d4b22efa)
<br>
<hr>
<br>

### ✔ 회원 가입시 토큰 생성
- 회원가입시 token 부분을 공란으로하여 서버로 보내고 서버에서는 userToken에 토큰을 생성하여 맵핑한다.

-frontend / useAuth.js
```javascript
// 회원 가입
const postSignUp = (id, pw, email, phone) => {
  axios.post(`${baseUrl}/post/user/signUp`, {
    userId: id,
    userPw: pw,
    userEmail: email,
    userPhone: phone,
    userToken: ``
  }).then(res => {
    console.log("전송 성공")
    if(res.data === 'fail') {
      window.alert("존재하는 아이디 입니다.")
    }
    console.log(res.data)
  }).catch(err => {
    error.value = err
    console.log(error.value)
  })
}
```
<br>
<br>

- backend / controller
```java
// 회원가입
@PostMapping("/post/user/signUp")
  public String postSignUp(@RequestBody UserDto vo) {
  return userService.postSignUp(vo);
}
```
<br>
<br>

- service 생략

- UserServiceImpl
  - 회원가입 시키기 전 DB에서 Seq, Id, Token 값이 1개라도 똑같은 것이 있으면 회원가입이 실패한다.
  - 현재 3개가 PK로 설정되어있기 때문이다.
  - 통과됐으면 만들어놓은 `getToken` 함수를 통해 토큰을 생성한다.
  - 토큰의 key, value는 `id`, `로그인한 id`로 준다. 
  - 이제 공란으로 넘어왔던 UserToken 컬럼에 토큰을 부여한다.

```java
// 회원가입
@Override
public String postSignUp(UserDto vo) {
  List<UserDto> list = userRepository.findAll();
  try {
	  if(list.stream().noneMatch(d -> 
		  d.getUserSeq().equals(vo.getUserId()) ||
		  d.getUserId().equals(vo.getUserId()) ||
		  d.getUserToken().equals(vo.getUserToken()))) {
		  String token = jwtService.getToken("id", vo.getUserId());
		  vo.setUserToken(token);
		  userRepository.postSignUp(vo);
		  System.out.println("회원가입 성공");
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
<br>
<br>

- JwtServiceImpl
```java
// JWT 비밀키
private String secretKey = "abvcbcxsdfjoijsdoifj2374iluhaf823fa28j3fla982j3fl9a823jf@##$!$jaiowef";

// JWT 생성 메서드
@Override
public String getToken(String key, Object value) {

  Date expTime = new Date();
  expTime.setTime(expTime.getTime() + 1000 * 60 * 5);
  byte[] secretBytekey = DatatypeConverter.parseBase64Binary(secretKey);
  Key signKey = new SecretKeySpec(secretBytekey, SignatureAlgorithm.HS256.getJcaName());
  
  Map<String, Object> headerMap = new HashMap<>();
  headerMap.put("typ", "JWT");
  headerMap.put("alg", "HS256");
  
  Map<String, Object> map = new HashMap<>();
  map.put(key, value);
  
  JwtBuilder builder = Jwts.builder().setHeader(headerMap).setClaims(map).setExpiration(expTime).signWith(signKey,
		  SignatureAlgorithm.HS256);
  
  return builder.compact();
}
```
<br>
<hr>
<br>

### ✔ 서버에서 id, pw를 조회하여 검증
- 클라이언트에서 보낸 id, pw 데이터를 DTO에 담는다.

- 담은 데이터를 DB로 보내 id가 있는지 확인한다.

- frontend / useAuth.js
  - 로그인이 성공하면 store에 설정되어 있는 `account`에 리턴 데이터를 담는다.

```javascript
// JWT 로그인
const login = async (id, pw) => {
  await axios.post(`${baseUrl}/post/user/jwtLogin`, {
    userId: id,
    userPw: pw
  })
  .then(res => {
    account.value.id = res.data
    console.log("토큰 id : ", res.data)
    window.alert("로그인 하였습니다.")
  }).catch(err => {
    error.value = err
    console.log(error.value)
    window.alert("로그인 실패입니다.")
  })
}
```

- backend / controller
```java
// JWT 로그인
@PostMapping("/post/user/jwtLogin")
  public ResponseEntity memJWTLogin(@RequestBody UserDto vo, HttpServletResponse res) {
  return jwtService.memJWTLogin(vo, res);
}
```

- service 생략

- JwtServiceImpl
  - 클라에서 넘어온 유저데이터를 통해 DB에 회원이 있는지 확인한다.
  - 회원 정보가 확인됐으면 쿠키에 value값으로 DB에 있는 해당 유저의 token 값을 맵핑한다.
  - 해당 회원은 자신이 회원가입했던 token 값을 쿠키로 받아 로그인에 성공한다.

```java
// JWT 로그인
@Override
  public ResponseEntity memJWTLogin(UserDto dto, HttpServletResponse res) {
  UserDto mvo = userRepository.memLogin(dto);
  if (mvo != null) { // 로그인 성공
	  String id = mvo.getUserId();
	  Cookie cookie = new Cookie("token", mvo.getUserToken());
	  cookie.setHttpOnly(true);
	  cookie.setMaxAge(60 * 60);
	  res.addCookie(cookie);
	  
	  return new ResponseEntity<>(id, HttpStatus.OK);
  } else {
	  throw new ResponseStatusException(HttpStatus.NOT_FOUND);
  }
}
```
<br>
<hr>
<br>

### ✔ 새로고침으로 인한 쿠키 풀림
- store에 저장된 전역 데이터는 브라우저가 새로 고침되면 초기화 된다.

- 때문에 쿠키 값은 지속시간 설정 때문에 살아 있지만 store에 저장된 데이터가 날라가서 로그인이 풀리게 된다.

- 이를 방지하기위해 App.vue에서 쿠키를 통한 토큰 id를 체크하여 store에다가 데이터를 지속적으로 유지시켜준다.

- App.vue는 vue가 실행되면 가장 먼저 렌더링 되는 곳이기 때문에 이곳에 설정 한 것.

- 토큰 체크 시 브라우저에서 가져온 토큰 값을 이용해 DB에서 userId값을 체크하는 유효성 검증을 한다.
<br>
<br>

- frontend / App.vue
```javascript
setup() {
  // 새로고침해도 세션 및 쿠키 체크를 통해 로그인을 유지한다.
  const { check } = useAuth();
  check()
}
```
<br>
<br>

- composables / useAuth.js
```javascript
// JWT 체크
const check = async () => {
  await axios.post(`${baseUrl}/post/user/checkJwt`)
  .then(res => {
    account.value.id = res.data
  }).catch(err => {
    error.value = err
    console.log(error.value)
  })
}
```
<br>
<br>

- backend / controller
  - `@CookieValue` 어노테이션을 통해 쿠키 값을 브라우저에서 가져온다.

```java
// JWT 체크
@PostMapping("post/user/checkJwt")
  public ResponseEntity checkJwt(@CookieValue(value = "token", required = false) String token) {
  return jwtService.checkJwt(token);
}
```
<br>
<br>

- service 생략

- JwtServiceImpl
  - 브라우저에서 받은 token 값을 Claims 객체를 이용해 token을 생성할 때 부여한 value 값을 뽑아낸다.
  - 회원가입시 key : id, value : 로그인한 id 로 부여하였다. 즉, 로그인했던 id가 value로 추출
  - 얻은 id값으로 DB에 보내 실제 회원에게 부여한 토근이 맞는지 검증한다.
  - 회원에게 부여했던 토큰과 브라우저에서 받은 토큰이 일치하다면 토큰의 value인 회원 ID를 다시 리턴한다.

```java
// JWT 로그인 체크
@Override
public ResponseEntity checkJwt(String token) {
  Claims claims = getClaims(token);
	    if (claims == null) {
	        return new ResponseEntity<>("Invalid token", HttpStatus.BAD_REQUEST);
	    }
	   
   // token 생성시 key를 "id"로 하였기 때문에, key를 통해 value를 얻는다.
  // Stirng id = 여기에 token 생성 시의 value가 담기는 것 (= 로그인 ID)
  String id = claims.get("id").toString();
  
  // token의 value값을 이용해 db에서 해당 userId의 정보를 찾는다.
  UserDto dto = userRepository.findUserDetail(id);
  
  // 브라우저 쿠키 token 값과 DB의 token 값을 비교해 유효성 검증
  if (dto.getUserToken().equals(token)) {
	  return new ResponseEntity<>(id, HttpStatus.OK);
  }
  return new ResponseEntity<>(null, HttpStatus.OK);
}


// JWT 로그인 체크 메서드
public Claims getClaims(String token) {
  if (token != null && !"".equals(token)) {
	  try {
		  byte[] secretBytekey = DatatypeConverter.parseBase64Binary(secretKey);
		  Key signKey = new SecretKeySpec(secretBytekey, SignatureAlgorithm.HS256.getJcaName());
		  return Jwts.parserBuilder().setSigningKey(signKey).build().parseClaimsJws(token).getBody();
	  } catch (ExpiredJwtException e) {
		  // 토큰이 만료됨
	  } catch (JwtException e) {
		  // 유효하지 않을 때
	  }
  }
  return null;
}

```
<br>
<hr>
<br>

### 💡 JWT를 이용한 로그인 과정에서의 회고
- 기존 쿠키로 로그인을 하면 브라우저에서 쿠키의 key, value를 직접 조작해서 로그인이 가능했다.

- 쿠키는 보안성이 매우 취약하기 때문에 JWT를 쿠키에 담아서 로그인을 하니 value를 직접 입력하여 로그인이 되는<br>
불상사가 사라졌다.

- 이 때문에 쿠키+토큰을 통해 로그인을 해야한다.

- 하지만 토큰의 유효시간 설정 시, 유효기간이 끝난 후 토큰 체크를 하면 fail이 나게된다.

- 현 코드에서는 DB에 token을 담고 그 token을 이용하여 검증 로직을 진행하고 있는데, 어떻게 해야 할까...?
<br>
<hr>
<br>

## 2024-01-04 내용 추가
- 현재 회원가입 시 토큰값을 저장하고 있는데, 이는 잘못된 개념

- 로그인 할 때 마다, 토큰을 부여하는 방식으로 진행해야하고, 토큰 만료 시 마다 새로 발급해야한다.

- 또한 토큰은 비밀키가 암호화 된 것이기 때문에 내용 그대로 store에 담아 사용해도 상관은 없다
  - 토큰은 String이기 때문에 값 자체를 클라와 서버끼리 주고받아도 됨
