## use strict란 무엇인가?
- 'use strict'; 단일 구문으로 사용

- ES5부터 지원한다.

- 암묵적인 '느슨한 모드(sloppy mode)'를 해제하고, 명시적인 '엄격 모드(Strict Mode)'를 사용하는 방법

- JS의 제한된 버전을 선택함으로써, 런타임 시 JS코드에 대하여 의도치 않은 오류를 막아주는 수단

- 즉, 애매하거나 이상한 키워드 입력을 막아주는 것이다.
<br>
<hr>
<br>

### ✔ usc strict 사용으로 발생하는 제약 조건들
- 전역 변수를 허용하지 않으며, 전역 변수 선언시 오류가 발생한다.

- 변수 이름 선언 및 사용시 "var"가 누락되면 오류가 발생한다.

- 값 할당 실패는 오류가 발생 (NaN = 1;)

- 삭제할 수 없는 속성을 삭제하려고 하면 (Object.prototype 삭제) 오류가 발생

- 읽기 전용 속성에 쓰기를 하려고 하면 오류가 발생

- 객체 리터럴의 모든 속성 이름은 고유해야한다. (var x = {x1 : "1", x1 : "2"})

- 함수의 파라미터는 고유해야 한다. (function calc (x, x) {}와 같이 같은 파라미터 이름 중복 사용 금지).

- 8진수 구문과 8진수 이스케이프 표현 금지(var a = 010; 과 같은 8진수 표현 사용 금지)

- with 키워드 금지

- 'eval', 'agruments'는 예약어로 변수명으로 사용할 수 없음.(var eval = 1;)

- 변수를 생성하는 eval()은 보안상 사용할 수 없음. eval("var x = 2");)

- 변수 이름과 함수 이름 삭제 금지 (var a = 1; delete a;)

- arguments.callee 미지원. 
  - arguments.callee 속성은 이름이 없는 익명 함수(anonymous function)를 선언해서 사용할 때 실행 중인 함수 안에서 함수 자신을 참조하기 위해서 사용
  - 재귀 호출 방식으로 자신을 호출하는 방식은 잠재적으로 참조 오류를 발생시킬 수 있다.
<br>
<hr>
<br>

### ✔ 'use strict' 를 사용하는지 체크하기
- 전역 객체인 this와 자바스크립트 함수를 사용해 엄격 모드가 실행 중인지 확인할 수 있다.

- 전역 this는 엄격 모드에서 사용할 수 없기 때문에 this 가 사용 가능하면 일반 모드이고 true가 된다.
  - "!true"는 false가 반환하고 엄격 모드가 아님을 알려주게 된다.
<br>

```
function isStrictMode(){
    return !this;
}
```
<br>

- 리액트의 경우 strict 모드가 기본적으로 적용되어 있다.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/c1f86f70-0ea3-4842-ada4-780a1694c241)
<br>
<hr>
<br>

### ✔ 'use strict'를 사용한다고 더 안정적인 코드가 되지는 않는다.
- 'use stict'는 더 엄격하게 코드 사용 규칙을 제한하고, 잠재적으로 오류가 발생할 수 있는 전역 변수 사용을 제한하기 때문에 에러 발생 확률이 적다.

- 하지만 그렇다고 'use strict' 사용이 극적으로 더 안정적인 코드를 만들어주는 것은 아니다.

- 단지 잘못된 대입을 차단해주는 등의 문제를 피하는 것이 주요 기능이라고 보면 된다.
<br>
<hr>
<br>

### ✔ 이미 작성한 자바스크립트에 'use stict'를 사용하는 것은 피해야 한다.
- 이미 작성되있는 자바스크립트는 어떤 에러가 있을지 모르기 때문에 함부로 적용해서는 안된다.

- ex) 'use strict' 이 없는 경우 a는 10진수로 10, b는 10진수로 8이 되버린다.

```
var a = 10;
var b = 010;
```
<br>

- 리액트에서 적용하려고 하니 에러가 등장한다.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/5a63f46e-a9aa-4fb9-a86d-176f79df4bb2)
<br>
<hr>
<br>

**Reference**<br>

[APOST : 자바스크립트 엄격 모드('use strict';)의 개념과 제약 사항의 이해](https://apost.dev/972/)<br>
[devHagaa : [Javascript] use strict란?](https://velog.io/@sunkim/Javascript-use-strict%EB%9E%80)
