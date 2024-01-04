## vue router
### ✔ $router
- $router는 Router 인스턴스를 가리킨다.

- Router 인스턴스는 웹 애플리케이션 전체에서 딱 하나만 존재하는 것으로 전반적인 라우터 기능을 관리한다.

- router-link 없이 페이지를 이동할 때 쓰이게 된다.

- 스크립트에서는 $없이 사용가능하지만 vue 내부에서 사용하기 위해서는 $를 profix로 붙여야한다.
  - 그러면 Vue 인스턴스 내부에서 라우터 인스턴스에 $router로 액세스 할 수 있다.
<br>
<br>

- 사용 가능한 기능은 아래와 같다.

- script에서 사용 시 $를 제거해주면 그대로 사용이 가능하다.
```
// 페이지 이동
$router.push("/detail/0");

// 변수와 같이 진행
$router.push("/detail/" + index);

// 뒤로 한번 가기
$router.go(-1);

// 뒤로 두번 가기
$router.go(-2);

// 앞으로 한번 가기
$router.go(1);
```
<br>
<hr>
<br>

### ✔ $route
- 이와 달리 $route.params 등의 코드에 나오는 $route 는 Route 객체다.

- 페이지 이동 등으로 라우팅이 발생할 때마다 생성되며, 현재 활성화된 **라우트의 상태를 저장**한 객체이다.

- 현재의 경로 및 URL 파라미터 등의 정보를 이 객체에서 받을 수 있다.

- 사용 가능한 기능은 아래와 같다.

```
// 현재 경로
$path

// 쿼리 파라미터 확인
$params : 정의된 URL 패턴과 일치하는 파라미터의 키-값 쌍을 담고 있는 객체. 파라미터가 없다면 빈 객체.

// 현재 쿼리의 객체
$query : 쿼리 문자열의 키-쌍 값을 담고 있는 객체. 쿼리가 없다면 빈 객체. 
(경로가 /foo?user=1 이면 $route.query.user == 1이 된다.

// 해시값 확인
$hash : 현재 URL에 URL 해시가 있을 경우 라우트의 해시값을 갖는다. 해시가 없다면 빈 객체

// 쿼리 + 해시 전체를 포함한 URL
$fullPath

// 라우트에 이름이 있는 경우, 라우트의 이름
name
```
<br>
<hr>
<br>

### ✔ vue 인스턴스 내부에서 사용시 $를 붙여야 하는 이유
- Vue.js에서 $ 기호는 Vue 인스턴스의 내장 속성이나 메소드에 접근할 때 사용되는 표기법이다.

- 때문에 내장 속성을 활용하기 위해서는 $를 profix로 사용해야 되는 것이다.
<br>
<hr>
<br>

**Reference**<br>

[jhhan의 블로그 : Vue.js - vue router](https://jhhan009.tistory.com/111)<br>
[nc2u : [Vue Router :: Router 인스턴스와 Route 객체 비교](https://im-nc2u.tistory.com/entry/Vue-Router-Router-%EC%9D%B8%EC%8A%A4%ED%84%B4%EC%8A%A4%EC%99%80-Route-%EA%B0%9D%EC%B2%B4-%EB%B9%84%EA%B5%90)<br>
