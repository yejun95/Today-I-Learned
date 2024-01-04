## Springboot + Vue3를 이용한 Cookie 설정 
**기본 흐름**
- 클라이언트에서 axios를 통해 id, pw를 서버로 보낸다.

- 서버에서 DB를 조회하여 해당 id의 유/무를 파악하여 쿠키를 생성할지 Exception처리할지 결정한다.
  - DB쪽 코드는 생략한다.
<br>

- DB에 아이디가 있다면 클라이언트로 쿠키 id를 리턴한다.

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

### ✔ 클라이언트에서 서버로 id, pw 전송
- axios를 통해 `useAuth.js`의 `login` 함수를 이용하여 id, pw 전송

- store로 데이터를 담는다.
<br>
<br>

- composables / useAuth.js
```javascript
import axios from 'axios';
import { ref } from 'vue';
import { useUserStore } from '@/stores/user';
import { storeToRefs } from 'pinia';

export default function useAuth() {
  const store = useUserStore();
  const { account } = storeToRefs(store);
  const error = ref();
  const baseUrl = '/api'
  const userIdCheck = ref();

  // 세션 로그인
  const login = async (id, pw) => {
    await axios.post(`${baseUrl}/post/user/login`, {
      userId: id,
      userPw: pw
    })
    .then(res => {
      account.value.id = res.data  // 로그인 성공시 쿠키 아이디를 store에 저장
      window.alert("로그인 하였습니다.")
    }).catch(err => {
      window.alert("로그인 실패입니다.")
      error.value = err
      console.log(error.value)
    })
  }
}
```

- store / user.js
```javascript
import { defineStore } from "pinia";
import { ref, computed } from "vue";

export const useUserStore = defineStore("user", () => {

  const account = ref({
    id: null
  })

  return { 
    account,

  };
});
```
<br>
<hr>
<br>

### ✔ 서버에서 id, pw를 조회하여 검증
- 클라이언트에서 보낸 id, pw 데이터를 DTO에 담는다.

- 담은 데이터를 DB로 보내 id가 있는지 확인한다.

- controller
```java
// 쿠키 로그인
@PostMapping("/post/user/login")
  public String memLogin(@RequestBody UserDto vo, HttpServletRequest httpServletRequest) {
  return sessionService.memLogin(vo, httpServletRequest);
}
```
<br>
<br>

- CookieService
```java
// 쿠키 로그인
	public String memLogin(UserDto vo, HttpServletRequest httpServletRequest);
```
<br>
<br>

- CookieServiceImpl
```java
// 쿠키 로그인
@Override
public String memCookieLogin(UserDto dto, HttpServletResponse response) {
  UserDto user = userRepository.memLogin(dto);
  if(user != null) { // 로그인 성공
	  // 쿠키에 시간 정보를 주지 않으면 세션 쿠키가 된다. (브라우저 종료시 모두 종료)
	  // 쿠키 값은 문자열이여야 하기 때문에 id가 정수일경우 String.valueOf를 통해 문자열로 변환
	  // 현재는 스트링으로 되어있어서 쓰지 않아도 무방하다.
	  Cookie cookie = new Cookie("memId", String.valueOf(dto.getUserId()));
	  
	  // 쿠키를 전송할 도메인 지정
	  // 도메인 앞에 `.`을 붙이면 해당 주소를 포함한 하위 도메인에서도 사용 가능
	  // ex) .naver.com -> shop.naver.com, news.naver.com 다 가능
	  // 쿠키는 정해진 도메인 영역에서만 유효하다.
	  // 별도의 명시하지 않으면 기본값으로 쿠키를 보낸 서버의 도메인으로 설정된다.
	  cookie.setDomain("localhost");
	  
  //     cookie.setPath("/"); // 모든 경로에서 쿠키 사용
	  cookie.setHttpOnly(true); // 스크립트가 쿠키에 접근하는 것을 막음
  //	  cookie.setSecure(false); // HTTPS 사용시에만 쿠키 정보 전달
	  cookie.setMaxAge(60 * 60); // 1시간
	  response.addCookie(cookie);
	  return user.getUserId();
  } else { // 로그인 실패
	  throw new ResponseStatusException(HttpStatus.NOT_FOUND);
  }
}
```

- `return user.getUserId();` 해당 리턴으로 인하여 클라이언트쪽 store에 값이 저장된다.
  - `store / user.js`에서 account 라는 변수명으로 ref 선언되어있음
<br>

- 로그인 성공 시, Response Headers / Set-Cookie 에 쿠키가 담겨온다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/7e1bc33e-d63a-4d1b-9ec1-79a880fee374)
<br>
<br>

**쿠키 구조**
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/8256a301-4cde-4edb-a8b6-61a8b26bbbee)
<br>
<br>

- 쿠키 로그인 코드에서 `cookie.set`으로 시작되는 것들에 대한 설명이다.

- Expires
  - 쿠기가 언제 삭제되는지 결정
  - Max-Age를 통해 지정된 만료일이 되면 디스크에서 쿠키가 제거
<br>

- Domain
  - 쿠키가 사용하는 도메인 지정
  - ex) domain=naver.com
    - .naver.com 처럼 앞에 '.'을 붙여주면 하위 도메인까지 커버 가능
    - ex) shop.naver.com / blog.naver.com
    - 도메인이 달라도 유효한 인증자인지 알 수 있는 이유
  - 이 값이 현재 탐색 중인 도메인과 일치하지 않을 경우, "타사 쿠키"로 간주되며 브라우저에서 거부.
    - 이렇게 하여 한 도메인에서 다른 도메인에 대한 쿠키를 사용하지 못하게 설정
    - 다른 사이트에서 같은 key 값으로 쿠키를 삽입하여도 막아주는 것이다.

아래는 도메인이 다른 네이버의 예시이다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/f48d8818-0347-48fe-b967-da7a273275d9)
<br>
<br>

- Path
  - 쿠키를 반환할 경로를 결정
  - ex) path=/
  - 도메인의 루트 경로로 이동할 경우 쿠키가 전송
<br>

- Secure
  - 보안 연결 설정
  - true 설정 시 Https일 때만 쿠키가 전송된다.
<br>

- HttpOnly
  - 스크립트로 쿠키에 접근하는 것을 막아준다.

httponly 속성을 적용하지 않으면 스크립트로 쿠키 탈취가 가능하다.
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/efb5f4c0-bcd0-496f-8995-110ddca21bc5)

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/da176dd1-d17c-49b0-9363-ad498eeb7ab1)
<br>
<br>

- httponly 속성 추가 시 스크립트로 탈취 불가능
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/72d71664-310e-4686-8c7e-b983e35d9080)

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/663cba61-f0af-4ee0-a50e-877b457c31e4)
<br>
<hr>
<br>

### ✔ 새로고침으로 인한 세션 풀림
- store에 저장된 전역 데이터는 브라우저가 새로 고침되면 초기화 된다.

- 때문에 쿠키 값은 지속시간 설정 때문에 살아 있지만 store에 저장된 데이터가 날라가서 로그인이 풀리게 된다.

- 이를 방지하기위해 App.vue에서 쿠키 id를 체크하여 store에다가 데이터를 지속적으로 유지시켜준다.
  - App.vue는 vue가 실행되면 가장 먼저 렌더링 되는 곳이기 때문에 이곳에 설정 한 것.
<br>

- 쿠키 체크 시 브라우저에서 가져온 쿠키 값을 이용해 DB에서 userId값을 체크하는 유효성 검증을 한다.
<br>
<br>

- App.vue
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
// 쿠키 체크
const check = async () => {
  await axios.post(`${baseUrl}/post/user/checkCookie`)
  .then(res => {
    account.value.id = res.data
    console.log("쿠키 id : ", res.data)
  }).catch(err => {
    error.value = err
    console.log(error.value)
  })
}
```
<br>
<br>

- backend, controller
```java
// 쿠키 체크
@PostMapping("post/user/checkCookie")
  public String checkCookie(HttpServletRequest req) {
  return cookieService.checkCookie(req);
}
```
<br>
<br>

- SessionService 생략

- SessionServiceImpl
```java
// 쿠키 체크
public String checkCookie(HttpServletRequest req) {
 // 해당 사이트에서 유효한 쿠키 확인
    // 쿠키에서 사용자 ID를 가져옴
 Cookie[] cookies = req.getCookies();
 
    // 쿠키가 있는지 확인
    if (cookies != null) {
        for (Cookie cookie : cookies) {
            if ("memId".equals(cookie.getName())) { // 쿠키의 이름이 "memId"인 경우
                String cookieId = cookie.getValue();
                String user = userRepository.findUser(cookieId);
                if(user != null) {
                 return cookieId;
                }
            }
        }
    } 
    throw new ResponseStatusException(HttpStatus.UNAUTHORIZED, "쿠키가 만료되었습니다.");
}
```
<br>
<hr>
<br>

### 💡 쿠키를 이용한 로그인 과정에서의 회고
**같은 아이디로 로그인 하였을 때, 쿠키 값이 항상 일정하다.**
- 쿠키 값을 UUID 등을 통해 랜덤값으로 삽입하면 사용자를 식별하기가 어려워진다.

- 그러면 누군가 브라우저에서 고정된 쿠키 값을 알게 되어 직접 삽입하면 로그인이 뚫리게 된다.

- 이 때문에 쿠키로는 민감정보를 절대로 다루지 않는다.

- 또한 이러한 단점을 해결하기 위해 Token 방식을 사용해야 된다.

- 현재 프로젝트에서는 memId라는 key와 로그인한 userId를 쿠키 값으로 제공한다.
  - 아래 부분에 쿠키 속성에서 직접 key, value를 직접 입력하면 로그인이 되버린다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/74cf276c-182d-4c13-b1c7-a65729d95f66)
<br>
<br>

**다른 브라우저에서 쿠키를 등록하면 내 도메인에서 로그인이 되는가?**
- 쿠키는 파일로 저장되기 때문에 다른 브라우저에서 나의 로그인 정보를 쿠키로 등록할 수 있지 않은가?

- 이는 cookie의 domain 속성을 통해서 막을 수 있다.

- `setDomain`을 통해 이 값이 현재 탐색 중인 도메인과 일치하지 않을 경우, "타사 쿠키"로 간주되며 브라우저에서 거부한다.

- 이렇게 하여 한 도메인에서 다른 도메인에 대한 쿠키를 사용하지 못하게 설정할 수 있다.

- 따로 설정하지 않으면 자동으로 기본값으로 쿠키를 보낸 서버의 도메인으로 설정된다.
<br>
<br>

**해당 사이트에서 유효한 쿠키는 어떻게 확인할 것인가??**
- 클라이언트가 요청을 보낼 때, 포함하는 쿠키를 서버는 어떻게 확인해야 하는가?

- 애초에 클라이언트에서 요청을 보낼 때, headers에 항상 cookie를 담아서 보낸다.
  - 현 프로젝트에서 요청을 보내는 부분은 새로고침할 때, checkCookie 함수를 실행하는 부분이여서 이를 예시로 진행
  - 아래를 보면 checkCookie 함수를 실행했을 때, Request headers에 쿠키를 포함해서 보낸다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/49b4d1bf-21d4-408d-986d-367ced81479f)
<br>
<br>

- 이를 서버에서 HttpServletRequest의 getCookies라는 함수를 이용하여 가져와 검증하는 것이다 
  - 요청이 들어온 headers의 cookie를 getCookies 함수로 받고, 내가 발급했던 쿠키 key가 맞는지 확인
<br>

```
// HttpServletRequest

/**
 * Returns an array containing all of the <code>Cookie</code> objects the client sent with this request. This method
 * returns <code>null</code> if no cookies were sent.
 *
 * @return an array of all the <code>Cookies</code> included with this request, or <code>null</code> if the request
 *             has no cookies
 */
Cookie[] getCookies();
```
<br>

- getCookies를 통해 가져온 쿠키 key와 value를 이용해 유효성 검증을 진행한다.
  - 이때 value값을 이용하여 DB에서 userId가 있는지 한 번 더 체크
<br>
<hr>
<br>

### ✔ 쿠키 인증 과정
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/9cd1e325-0fc4-4b7d-a082-2767ed26f28b)

- 기존 쿠키 인증과정에는 DB를 갔다오는 2번 5번 과정이 없다.

- 해당 프로젝트에서는 DB에 회원정보를 저장하고 있기 때문에 2번 로직이 포함된다.

- 또한 클라에서 데이터 요청 시, 인증된 사용자가 맞는지 확인하는 절차를 가진다.
  - 쿠키는 key, value로 발급되는데, 현 프로젝트에서는 value를 로그인한 id 값을 부여하였다.
  - 클라가 데이터 요청시 쿠키에 자신에 id값이 value로 담겨온다.
  - 서버는 클라의 id 값인 쿠키의 value를 db에서 조회하여 인증된 사용자인지 확인한다.
  - 클라 id == DB id 가 일치하다면 응답해준다.
