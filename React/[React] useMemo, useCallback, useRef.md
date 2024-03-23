## useMemo, useCallback, useRef란?
### ✔ useMemo
- Memoized **value**를 리턴하는 Hook
  - Memoization : 컴퓨터 분야에서 최적화를 위해서 사용하는 개념
  - 비용이 높은, 즉 연산량이 많은 함수를 저장해놓았다가 같은 입력 값으로 함수를 호출하면 새 함수를 호출하는 것이 아니고
  이전에 저장해놓은 값을 바로 반환한다.
  - 이를 통해 값을 빨리 리턴받을 수 있고, 비용을 줄일 수 있다.
<br>

- 쉽게 말해서 메모를 해두었다가 나중에 다시 사용하는 것.
  - Memoization == 메모 로 기억하자
<br>

**useMemo 사용법**

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/7ee775df-0ad4-44e0-8b6e-8717705aa69b)
<br>
<br>

- 의존성 배열에 들어있는 변수가 변했을 경우에만 새로 create 함수(useMemo)를 호출하여 결과값을 반환한다.

- 그렇지 않으면 기존 값을 그대로 반환한다.

- 이렇게 하면 컴포넌트가 다시 렌더링 될 때 마다 연산량이 높은 작업의 반복을 피할 수 있다.

- 결과적으로 빠른 렌더링 속도를 얻을 수 있다.

- 💡 주의점 : useMemo Hook 사용 시 useMemo로 전달된 함수는 렌더링이 일어나는 동안 실행된다.
  - 즉, 렌더링이 일어나는 동안 해서는 안될 작업을 useMemo Hook으로 사용하면 안된다.
  - Ex) useEffect에서 실행되는 사이트 이펙트들 (api 호출, 수동 dom 변경 등)
<br>

- 아래와 같이 의존성 배열을 넣지 않은 경우, 매 렌더링마다 함수가 실행된다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/5a03c2db-62ec-49ba-9149-f9c41162326c)
<br>
<br>

- 아래와 같이 의존성 배열에 빈 배열을 넣는 경우, 컴포넌트 마운트 시에만 호출된다.
  - 첫 마운트시에만 값을 계산하는 경우라면 이렇게 써도 된다.
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/f99378c4-60fe-45e3-9887-a8a24530259c)
<br>
<hr>
<br>

### ✔ useCallback
- useMemo() Hook 과 유사하지만 값이 아닌 함수를 반환한다.

- useMemo() Hook 과 동일하게 의존성 배열의 값이 바뀐 경우에만 함수를 재정의하여 리턴한다.

- 아래와 같이 의존성 배열의 값이 변경되면 콜백함수를 반환한다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/b6e60d4f-988d-418e-8081-1e2dc2833210)
<br>
<br>

**사용 이유**
- 일반적으로 함수를 정의하면 컴포넌트가 렌더링 될 때 마다 함수가 재정의되지만 useCallback Hook을 사용하면
의존성 배열의 값이 변경된 경우에만 함수를 재정의하여 리턴하는 것이다.

- 결국 불필요한 렌더링을 줄이기 위해 사용하는 것
<br>
<br>

**예시**
- 아래와 같이 useCallback() Hook 을 사용하지 않고 `handleClick` 이라는 함수를 정의하면 재렌더링마다 함수가 정의된다.

- 이렇게 되면 부모 컴포넌트가 렌더링 될 때 자식 컴포넌트도 같이 재렌더링 되면서 쓸데없이 props 값이 넘어간다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/95c8a83b-0a0b-473c-96e3-2169ec169060)
<br>
<br>

- 하지만 아래와 같이 useCallback() Hook을 사용하면 의존성 배열의 값이 변경될 때만 자식 컴포넌트도 재렌더링이 된다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/4b5d51e1-0c7f-4798-b097-7daaea3bf728)
<br>
<hr>
<br>

### ✔ useRef
- Reference를 사용하기 위한 Hook

- 리액트에서의 Reference는 특정 컴포넌트에 접근할 수 있는 객체를 말한다.
 
- 이때 useRef() Hook 은 Reference 객체를 반환하는 것이다.

- `refObject.current` 라는 속성을 이용하면 현재 참조하고 있는 Element를 가리킨다.
<br>
<br>

**사용법**
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/edc78e21-4418-43bd-9b73-0b9656e7ff85)

- 만약 초기값이 null이라면 초기값이 null인 Reference 객체를 반환하게 되는 것이다.

- 컴포넌트가 언마운트 되기 전까지는 유지된다.
<br>
<br>

**예시**
- 아래 예시는 버튼 클릭 시 input에 focus가 가는 로직이다. 

- input에다가 ref를 객체를 맵핑하고 `current` 속성을 이용하여 접근하게 된다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/ed74c52c-59dc-49dd-ae9f-9e17575f91b4)
<br>
<br>

- 💡 useRef() Hook은 내부 데이터가 변경되었을 때, 별도로 알리지 않는다.
  - 때문에 current 속성을 변경한다고 해서 재렌더링이 일어나지 않는다.
<br>

- 따라서 Ref에 DOM 노드가 연결되거나 분리되었을 경우, 어떠한 코드를 실행하고자 한다면 `Callback ref`를 사용해야한다.
<br>
<hr>
<br>

### ✔ Callback ref
- useCallback() Hook 과 태그에 맵핑하는 ref 속성을 결합한 것이다. 

- useRef() Hook의 current 속성을 변경해도 재렌더링이 안되기 때문에 사용하는 것

- 아래 그림처럼 useCallback() Hook을 사용하면서 태그에 ref 속성을 맵핑한다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/5a4dd48c-0eff-437e-bb1d-116f9cf96b8f)
<br>
<br>

- 이렇게 되면 H1의 높이 값을 매번 업데이트 하게 된다.
<br>
<hr>
<br>

**Reference**<br>

[인프런 Inje Lee : 처음 만난 리액트(React)](https://www.inflearn.com/course/%EC%B2%98%EC%9D%8C-%EB%A7%8C%EB%82%9C-%EB%A6%AC%EC%95%A1%ED%8A%B8/dashboard)
