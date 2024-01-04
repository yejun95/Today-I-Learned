## Springboot와 Vue3를 통한 Session 로그인
**기본 흐름**
- 클라이언트에서 axios를 통해 id, pw를 서버로 보낸다.

- 서버에서 DB를 조회하여 해당 id의 유/무를 파악하여 세션을 생성할지 Exception처리할지 결정한다.
  - DB쪽 코드는 생략한다.
<br>

- DB에 아이디가 있다면 클라이언트로 세션 id를 리턴한다.

- 클라이언트로 리턴된 세션 id를 store(pinia)에 저장하여 홈페이지 전체에서 로그인을 유지한다.

- 새로고침하면 세션이 풀리기 때문에 App.vue에서 세션을 지속적으로 체크하는 함수를 삽입한다.
  - 해당 함수 실행 시, 서버에서 세션 id를 확인하여 해당 값이 있으면 동일한 데이터를 다시 리턴한다.
  - 때문에 store에 세션 값이 지속적으로 유지된다.
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
```
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
      account.value.id = res.data  // 로그인 성공시 세션아이디를 store에 저장
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
```
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
```
// 세션 로그인
@PostMapping("/post/user/login")
  public String memLogin(@RequestBody UserDto vo, HttpServletRequest httpServletRequest) {
  return sessionService.memLogin(vo, httpServletRequest);
}
```
<br>
<br>

- SessionService
```
// 세션 로그인
	public String memLogin(UserDto vo, HttpServletRequest httpServletRequest);
```
<br>
<br>

- SessionServiceImpl
```
// 세션 로그인 구현
@Override
public String memLogin(UserDto vo, HttpServletRequest request) {
  UserDto user = userRepository.memLogin(vo);
  if(user != null) { // 로그인 성공 => 세션 생성
	  // 세션을 생성하기 전에 기존의 세션 파기
	  request.getSession().invalidate();
  
	        // 세션이 없으면 생성
	        HttpSession session = request.getSession(true);
	        
	        // 세션에 userId를 넣어줌
	  session.setAttribute("userId", user.getUserId());
	  
	  // 세션을 30분 동안 유지
	  session.setMaxInactiveInterval(1800);
	  
	  return user.getUserId();
  } else { // 로그인 실패
	  // http 응답코드를 리턴한다.
	  // DB에 정보가 없을 시 404 리턴
	  throw new ResponseStatusException(HttpStatus.NOT_FOUND);
  }
}
```

- `return user.getUserId();` 해당 리턴으로 인하여 클라이언트쪽 store에 값이 저장된다.
  - `store / user.js`에서 account 라는 변수명으로 ref 선언되어있음
<br>

- 로그인 성공 시, Response Headers / Set-Cookie 에 세션이 담겨온다.
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/2954e96a-11e2-4941-8a61-415fa3e9ce11)
<br>
<hr>
<br>

### ✔ 새로고침으로 인한 세션 풀림
- store에 저장된 전역 데이터는 브라우저가 새로 고침되면 초기화 된다.

- 때문에 세션 값은 살아 있지만 store에 저장된 데이터가 날라가서 로그인이 풀리게 된다.

- 이를 방지하기위해 App.vue에서 세션 id를 체크하여 store에다가 데이터를 지속적으로 유지시켜준다.
  - App.vue는 vue가 실행되면 가장 먼저 렌더링 되는 곳이기 때문에 이곳에 설정 한 것.
<br>
<br>

- App.vue
```
  setup() {
    // 새로고침해도 세션 및 쿠키 체크를 통해 로그인을 유지한다.
    const { check } = useAuth();
    check()
  }
```
<br>
<br>

- composables / useAuth.js
```
// 세션 체크
const check = async () => {
  await axios.post(`${baseUrl}/post/user/checkSession`)
  .then(res => {
    account.value.id = res.data
    console.log("세션 id : ", res.data)
  }).catch(err => {
    error.value = err
    console.log(error.value)
  })
}
```
<br>
<br>

- backend, controller
```
// 세션 체크
@PostMapping("post/user/checkSession")
  public String checkSession(HttpSession session) {
  return sessionService.checkSession(session);
}
```
<br>
<br>

- SessionService 생략

- SessionServiceImpl
```
  // 세션 체크
  public String checkSession(HttpSession session) {
      // 사용자가 login 데이터를 요청
   // 서버에서 해당 쿠키를 검증하기 위해 session.getAttribute로 세션Id를 가져옴
      String user = (String) session.getAttribute("userId");
      System.out.println(user);

      // 쿠키에서 넘어온 세션 Id 유효성 검증
      if (user != null) {
       System.out.println("유효성 검증 통과");
          return user;
      } else {
          // 세션에 사용자 ID가 없는 경우 401 리턴
          throw new ResponseStatusException(HttpStatus.UNAUTHORIZED, "세션이 만료되었습니다.");
      }
  }
```
<br>
<hr>
<br>

### 💡 서버에서 해당하는 브라우저의 세션을 알 수 있는 이유
- 서버에서 getAttribute를 통해 세션 Id를 가져오면서 재검증을 하는 로직인데, 어떻게 해당 사이트의
세션 Id인지 알고 요청을 처리하는 것일까?

- 이는 브라우저에서 서버로 요청을 보낼 때, 자신의 세션 Id를 Request Headers의 Cookie 속성에 담아서 보내기 때문이다.

- checkSession() 함수 실행 시 HTTP 속성 확인
  - 브라우저에서 새로고침을 하면 checkSession이 실행된다.
  - 해당 함수에 들어있는 HTTP 요청을 보면 Request Headers / Cookie에 세션 값을 보내고 있다.
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/8bd5704e-bf3e-4bf6-a2fc-4aa3e0883611)
<br>
<br>

- 이를 HttpSession 클래스의 getAttribute 함수를 이용하여 가져 올 수 있는 것이다.
  - HttpSession 내부 로직을 살펴보면 getAttribute 함수의 설명을 볼 수 있다.
  - Returns the object bound with the specified name in this session, or <code>null</code> if no object is bound under the name.: 현재 세션에 지정된 이름으로 바인딩된 객체를 반환하며, 해당 이름으로 바인딩된 객체가 없으면 null을 반환합니다.
  
  - @param name: 메서드에 전달되는 매개변수인 name은 객체의 이름을 나타냅니다.
  - @return the object with the specified name: 지정된 이름을 가진 객체를 반환합니다.
  - @exception IllegalStateException if this method is called on an invalidated session: 이 메서드가 무효화된 세션에서 호출되면 IllegalStateException 예외가 발생합니다.
```
    /**
     * Returns the object bound with the specified name in this session, or <code>null</code> if no object is bound
     * under the name.
     *
     * @param name a string specifying the name of the object
     *
     * @return the object with the specified name
     *
     * @exception IllegalStateException if this method is called on an invalidated session
     */
```
<br>
<hr>
<br>

### ✔ 세션 인증 방식
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/6e8d78a9-1623-4e61-b8c1-97b5602c9944)

- 톰캣을 사용할 경우 세션 ID는 JSESSIONID라는 이름으로 발급된다.

- 7번 쿠키 검증 시 @CookieValue를 쓰지 않아도 Httpsession 클래스를 통해 세션 정보를 획득 할 수 있다.
