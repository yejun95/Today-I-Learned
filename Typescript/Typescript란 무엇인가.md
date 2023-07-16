# TypeScript란?
- JavaScript에 타입을 부여한 언어로 JavaScript의 확장된 언어이다.
이 말이 무엇인가? 밑에 코드를 통하여 알아보자.
```javascript
function sum(a, b) {
  return a + b;
}

function sum(a: number, b: number) {
  return a + b;
}
```
- 일반적으로 sum을 쓴다는 것은 숫자를 더해주겠다는 뜻으로 받아들이는 경우가 있는데, 자바스크립트의 경우
문자열 + 문자열 or 숫자 + 문자열 더하기 등이 가능해 시스템 에러를 발생 시킬 수가 있다. 이같은 상황을 방지하기 위해
Type을 명시하여 적어주는 것이다.
- 앞서 말한 에러가 나는 상황의 예를 확인해보자.
```javascript
sum(10, 20); // 30

sum('10', '20'); // 1020
```
- Type을 명시해주지 않으면 문자열로 더하였을 때 에러없이 그대로 진행이 된다. 
이를 Typescript에서는 아래와 같이 보여준다.
```javascript
function sum(a: number, b: number) {
  return a + b;
}
sum('10', '20'); // Error: '10'은 number에 할당될 수 없습니다.
```
- 해당 코드를 VScode에서 확인 하였을 때 예시.
![image](https://github.com/bjsystems/rnd/assets/121341413/e8a34086-f0fb-4a75-ba51-d22ae589e649)

- **장점**
    - 에러 사전 방지 : 컴파일 단계에서 오류를 포착 가능
    - 안정성
    - 협업용이성
    - Typescript가 유추를 통해 알아서 타입을 변환하는 부분도 존재 ex) 비교 값이기 때문에 age를 number로 판단
    ```javascript 
    function isAdult(age) {
      return age > 18;
  }
    ```

- **단점**
    - 브라우저와 Node.js는 Typescript를 지원하지 않기 때문에 다시 Javascript파일로 컴파일하는 과정을 거쳐야 한다.
    단 현재 Node.js 모듈에서 사용자들이 적용해놓은 것들은 types@모듈명을 통해 사용가능 (그 외는 직접 찾아서 수기 작성)
    컴파일 방법
    - 가독성이 떨어진다. : Type 작성으로 인한 코드의 길어짐과 TypeError로 인한 빨간줄 대거 등장
    - 컴파일 방법 : `npx tsc` / 특정 파일만 컴파일 : `npx tsc test.ts`
 
[참조 : Typescript 핸드북](https://joshua1988.github.io/ts/why-ts.html#%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%9E%80)
