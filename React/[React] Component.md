## 컴포넌트란 무엇인가?
- 리액트에서 컴포넌트는 Vue.js와 동일하게 앱을 이루는 가장 작은 조각이며, 이 조각들이 모여
페이지를 구성하게 된다.

- 공통화하여 코드를 최소화하는 것에 그 목적을 두고 있다.
<br>
<hr>
<br>

### ✔ 종류
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/c5899e48-3690-4378-9513-a374dbcbb08d)

- 종래에는 Class Component를 사용하였지만, 코드가 불필요하게 길어지고 사용이 불편하다.

- 때문에 최근 추세는 Function Component를 사용하고 있다.
<br>
<hr>
<br>

### ✔ 코드 비교
**Function Component**
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/6a07f675-b567-4276-9928-9c31968d4d88)
<br>
<br>

**Class Component**
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/ca6695b8-4a28-4b1e-a8c5-f89a939e694f)
<br>
<br>

- Function Component와는 다르게 무조건 React.Component를 사용해야 한다.

- 또한 `render()` 함수를 초반에 실행해야하고, props에 접근하기 위해서 `this`를 붙힌다.

- 그 외에도, Class Component에서 사용가능한 state가 Function Component에서는 불가능하여 Hook 이라는 개념이
등장하였다.
<br>
<hr>
<br>

### ✔ 네이밍 규칙
- Component의 이름은 항상 대문자로 시작해야 한다.

- 대문자로 시작해야 리액트가 Component로 인식할 수 있다.

- 만약 소문자로 시작할 경우 리액트는 이를 태그로 인식한다.

- 아래 그림과 같이 태그와 컴포넌트는 소문자, 대문자로 구분된다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/c896f58a-0d55-4210-ac88-debd80c00f2d)
<br>
<hr>
<br>

**Reference**<br>

[인프런 Inje Lee : 처음 만난 리액트(React)](https://www.inflearn.com/course/%EC%B2%98%EC%9D%8C-%EB%A7%8C%EB%82%9C-%EB%A6%AC%EC%95%A1%ED%8A%B8/dashboard)
