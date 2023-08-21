## Tree Shaking 이란?
- 사용하지 않는 코드를 제거하는 방식 (Rollup, Webpack에서 찾아 볼 수 있다.)
- 나무를 흔들면 죽은 잎사귀들이 떨어지는 모습에 착안해 Tree Shaking이라고 명명되었음
<br>
<hr>
<br>

**✔ 등장 배경**
- 서비스의 규모가 커짐에 따라 코드도 방대해지는데, 번들 파일을 최적화 하여<br>
로드할 때 시간을 줄이기 위함이다.
<br>
<hr>
<br>

**✔ 사용 예시**
- 사용하는 모듈로부터 전체를 import 하지 않고 사용하는기능만 `{}` 부분적으로 import 한다.
- 밑에 코드를 보면은 App.js에서 food.js의 모듈을 가져올 때 필요한 부분만 `{}` 하게된다.<br>
=> `import { getPizza } from './food';
<br>

```javascript
// food.js
export const getPizza = () => {
    console.log('this is getPizza');
    return 'pizza';
}

export const getChicken = () => {
    console.log('this is getChicken');
    return 'chicken';
}
```
<br>

```javascript
// App.js
import * as React from 'react';
import { getPizza } from './food';

const App = () => {

    React.useEffect(() => {
        getPizza();
    }, [])

    return (
        <div>
            <h2>Tree Shaking Test...</h2>
        </div>
    )
}

export default App;
```
<br>
<hr>
<br>

**✔ babel 설정**

```javascript
"presets": [
    [
        "@babel/preset-env",
        {
            "modules": false
        }
    ],
],
```
<br>
<hr>
<br>

**Reference**<br>

[Sihus Log : 트리 쉐이킹이란 무엇인가?](https://sihus.tistory.com/46)<br>
[J4J Storage : Tree Shaking으로 최적화하기](https://jforj.tistory.com/166)
