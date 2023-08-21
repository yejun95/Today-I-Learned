## Flux 패턴이란?

- Flux는 사용자 입력을 기반으로 Action을 만들고 Action을 Dispatcher에 전달하여 Store(Model)의 데이터를 변경한 뒤<br>
View에 반영하는 단방향의 흐름으로 애플리케이션을 만드는 아키텍처입니다. 구조는 다음의 그림과 같습니다.
<br>

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/9fd5395c-3a74-4ee7-bb3e-2f4f69bf22da)
<br>
<hr>
<br>

**✔ 등장 배경**

- 대규모 애플리케이션에서 데이터 흐름을 일관성 있게 관리함으로써 프로그램의 예측가능성(Predictability)을 높이기 위함이다.

- 보편적으로 사용하던 MVC 패턴의 경우 규모가 커지게 되면 Model과 View의 상호작용이 서로 겹치게 되면서<br>
많은 의존성을 갖게 되어 예측하기가 힘들어진다.
<br>
<hr>
<br>

**✔ 구성 요소**

**Action**

- Action이란 데이터를 변경하는 행위로서 Dispatcher에게 전달되는 객체를 말한다.<br>
Action creator 메서드는 새로 발생한 Action의 타입(type)과 새로운 데이터(payload)를 묶어 Dispatcher에게 전달한다.
<br>

```javascript
{
  type: 'SET_PROFILE',
  data: {
    'name': 'Harry',
    'age': 458
  }
}
```
<br>

**Dispatcher**

- Dispatcher는 모든 데이터의 흐름을 관리하는 중앙 허브이며, Dispatcher에는 Store들이 등록해놓은 Action 타입마다의 콜백 함수들이 존재한다.

- Action을 감지하면 Store들이 각 타입에 맞는 Store의 콜백 함수를 실행하고 Store의 데이터를 조작하는 것은 오직 Dispatcher를 통해서만 가능하다.

- 또한 Store들 사이에 의존성이 있는 상황에서도 순서에 맞게 콜백 함수를 순차적으로 처리할 수 있도록 관리한다.
<br>

**Store (Model)**

- Store는 상태 저장소로서 상태와 상태를 변경할 수 있는 메서드를 가지고 있다.

- 어떤 타입의 Action이 발생했는지에 따라 그에 맞는 데이터 변경을 수행하는 콜백 함수를 Dispatcher에 등록한다.

- Dispatcher에서 콜백 함수를 실행하여 상태가 변경되면 View에게 데이터가 변경되었음을 알린다.
<br>

**View**

- View는 리액트 컴포넌트로 생각하면 되고, Store에서 View에게 상태가 변경되었음을 알려주면 최상위 View(Controller View)는 Store에서 데이터를 가져와 자식 View에게 내려 보낸다.

- 새로운 데이터를 받은 View는 화면을 리렌더링하고, 또한 사용자가 View에 어떤한 조작을 하면 그에 해당하는 Action을 생성한다.
<br>

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/1213aaec-b614-4bf2-9377-e46c447f5b50)
<br>

각 요소들은 단방향 흐름에 따라 순서대로 역할을 수행하고, View로부터 새로운 데이터 변경이 생기면 처음부터 다시 이 순서대로 실행한다.<br>
<br>

이렇게 함으로써 예외 없이 데이터를 처리할 수 있게 되었다.
<br>
<hr>
<br>

**Reference**<br>

[Harry's tech b.log : Flux 패턴이란?](https://velog.io/@andy0011/Flux-%ED%8C%A8%ED%84%B4%EC%9D%B4%EB%9E%80)<br>
[Flux](https://haruair.github.io/flux/docs/overview.html)<br>


