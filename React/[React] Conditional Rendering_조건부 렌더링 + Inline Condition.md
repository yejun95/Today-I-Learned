## 조건부 렌더링이란?
- Condition이란 조건, 상태 라는 뜻을 갖고 있다.

- 특정 조건에 따라 렌더링이 되는 것을 말한다.

- 예를 들면, if 문을 사용하여 true, false에 따라 return 하는 값을 다르게 해주는 것이 가장 기본이다.

- 또한 리액트에서는 컴포넌트 자체를 조건부 렌더링에 의해 보여줄 수 있다.
<br>
<hr>
<br>

### ✔ 예제
- 아래 두 함수는 태그를 리턴한다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/e65ded15-1b14-42b1-8d43-e6ea2df506ce)
<br>
<br>

- 조건부 렌더링에 의해 다른 컴포넌트에서 해당 컴포넌트들을 return 할 수 있다.

- 보여줄 화면을 손쉽게 바꿀 수 있다는 것이다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/d7d6bd78-030c-4ae4-b119-8f0bc17b4bdf)
<br>
<hr>
<br>

### ✔ Element Variable
- 조건부 렌더링을 사용하다보면 렌더링해야될 컴포넌트를 변수처럼 다뤄야 할 경우가 있는데, 이 때 사용한다.

- 한국말로 Element 변수라고 부른다.
<br>
<hr>
<br>

**예시**
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/42e19c50-4176-4c88-a6f6-d6c14861ad3e)
<br>
<br>

- 위 그림의 두 함수는 로그인 및 로그아웃 버튼을 만드는 함수이다.

- 이를 아래 그림처럼 조건부 렌더링을 이용하여 각각 다른 버튼을 보여줄 수 있다.
<br>
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/0f82065d-e62b-404b-8123-7f3dd06924f0)
<br>
<hr>
<br>

### ✔ Inline Conditions
- 라인의 안이라는 말이다.

- 해당하는 코드가 필요한 곳에 직접 집어넣는다는 뜻이다.

- 조건문을 코드 안에 집어넣는 것
<br>
<br>
 
**Inlie if**
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/21e30741-a3e7-48b9-a62d-cd66d891810a)
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/7d54770e-298e-4a61-8160-c2e367134487)
<br>
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/74a6680b-c482-4026-b482-21d950750e6a)
<br>
<br>

**Inline If-Else**
- 흔히 알고 있는 삼항연산자를 코드 안에서 사용하는 방식이다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/d27d5f6e-f9fa-46ae-8115-66b50d44d1b3)
<br>
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/13776615-3568-40aa-af45-b9b0e26febaf)
<br>
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/38ff8d9f-30ac-4bcc-bb53-9d57e9eb33a7)
<br>
<hr>
<br>

### ✔ 컴포넌트 렌더링 막기
- 컴포넌트를 사용할 때, If 조건을 사용하여 return null을 통해 렌더링을 막을 수 있다.
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/2ed133c1-b0a4-429f-90f8-d8f0c1ad68ae)
<br>
<br>

- 위 그림은 props.warning이 false 이면 null이 return되면서 `<div>경고!</div>` 해당 태그가 출력되지 않는다.

- props를 받고 있으니 자식컴포넌트라는 걸 알 수 있는데, 그렇다면 아래 그림에서 부모 컴포넌트도 확인해보자.
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/987046be-721b-4856-8842-513b06774067)
<br>
<hr>
<br>

**Reference**<br>

[인프런 Inje Lee : 처음 만난 리액트(React)](https://www.inflearn.com/course/%EC%B2%98%EC%9D%8C-%EB%A7%8C%EB%82%9C-%EB%A6%AC%EC%95%A1%ED%8A%B8/dashboard)
