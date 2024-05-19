## Shared State란 무엇인가?
- 부모와 자식 컴포넌트간 공유하는 state를 말한다.

- 동일한 value를 가지는 것이라고 보면 된다.
<br>
<hr>
<br>

### ✔ 부모-자식 컴포넌트간 state 공유 방법
- 부모 > 자식으로 value를 넘길 때, props로 넘긴다.
  - 값이 맵핑될 props
  - 자식에서 바꾼 값을 받아줄 props
  - 총 2개
<br>

- 자식에서는 props로 넘어온 값을 맵핑하고, 변경된 값을 onChange를 통해 함수를 적용하여 부모로 다시 올려 보낸다.
  - 이 과정에서 부모에서 설정한 '자식에서 바꾼 값을 받아줄 props`로 값을 넣어주는 것이다.
<br>

- 부모에서는 자식에서 props로 넘긴 값을 함수 or setState를 바로 사용해 실시간으로 값을 변경한다.

- 실제 코드로 알아보자.

**부모 컴포넌트 - Calculator.jsx**
```javascript
import React, { useState } from 'react';
import TemperatureInput from './TemperatureInput';

// 해당 함수 자체로 컴포넌트가 완성된 것.
function BoilingVerdict(props) {
    if (props.celsius >= 100) {
        return <p>물이 끓습니다.</p>
    }
    return <p>물이 끓지 않습니다.</p>
}

function toCelsius(fahrenheit) {
    return ((fahrenheit - 32) * 5) / 9;
}

function toFahrenheit(celsius) {
    return (celsius *9) / 5 + 32;
}

function tryConvert(temperature, convert) {
    const input = parseFloat(temperature);
    if (Number.isNaN(input)) {
      return "";
    }
    const output = convert(input);
    const rounded = Math.round(output * 1000) / 1000;
    return rounded.toString();
}

function Calculator(props) {
    const [temperature, setTemperature] = useState('');
    const [scale, setScale] = useState('c');
    
    const handleCelsiusChange = (temperature) => {
      setTemperature(temperature);
      setScale('c');
    }

    const handleFahrenheitChange = (temperature) => {
      setTemperature(temperature);
      setScale('f');
    }

    const celsius = scale === 'f' ? tryConvert(temperature, toCelsius) : temperature;
    const fahrenheit = scale === 'c' ? tryConvert(temperature, toFahrenheit) : temperature;
    
    return (
      <div>
          <TemperatureInput
              scale='c'
              temperature={celsius} // 기본 온도를 props로 보냄
               // 자식에서 올라온 값을 함수로 처리, setTemperature로 바로 사용가능하지만, setScale 때문에 함수 사용
              onTemperatureChange={handleCelsiusChange}
              />
          <TemperatureInput
              scale='f'
              temperature={fahrenheit}
              onTemperatureChange={handleFahrenheitChange}/>
          <BoilingVerdict
              celsius={parseFloat(celsius)}/>
      </div>
    )
}

export default Calculator
```
- 자식 컴포넌트인 TemperatureInput으로 scale, temperature, onTemperatureChange 3개의 props를 넘긴다.

- 💡 여기서 scale과 temperature는 단순 value를 넘기는 props지만, onTemperatureChange는 자식에서 올린 value를 재설정하는 부분이다.
<br>
<br>

**자식 컴포넌트 - TemperatureInput.jsx**
```javascript
const scaleNames = {
  c: '섭씨',
  f: '화씨'
};

function TemperatureInput(props) {
  const handleChange = (e) => {
      props.onTemperatureChange(e.target.value)
  };

  return (
    <fieldset>
        <legend>
            온도를 입력해 주세요(단위:{scaleNames[props.scale]}):    
        </legend>
        <input value={props.temperature} onChange={handleChange} />
    </fieldset>
  )
}

export default TemperatureInput;
```
- 부모에서 넘어온 scale, temperature, onTemperatureChange 3개의 props를 활용한다.

- 앞서 말했듯이 scale, temperature는 단순 value 맵핑이다.

- input 태그에서 onChange를 통해 부모에서 넘어온 onTemperatureChange props로 다시 값을 넘긴다.
  - 💡 이때 value를 맵핑할 때와 동일하게 props.{name} 으로 넘긴다.
<br>

- 그럼 부모 쪽에서는 아래의 onTemperatureChange를 통해 setTemperature를 변경하면서 state가 공유 되고, 실시간으로 값이 변경되는 것이다. 

```javascript
...
const handleFahrenheitChange = (temperature) => {
  setTemperature(temperature);
  setScale('f');
}
...

<TemperatureInput
    scale='f'
    temperature={fahrenheit}
    onTemperatureChange={handleFahrenheitChange}/>
...
```
> 자식에서 넘어온 값을 다시 setTemperature로 맵핑해주는 것이 중요 포인트
<br>

- 해당 프론트를 실행하여 브라우저에서 보면 다음과 같다

![GIF 2024-05-20 오전 12-21-44](https://github.com/yejun95/Today-I-Learned/assets/121341413/a384d436-a77c-4bfe-af79-15cab7485d18)
<br>
<hr>
<br>

### ✔ setTemperature에 바로 적용
- 현재 코드를 보면 `handleFahrenheiChange` 함수를 한번 거쳐서 적용하는 것이 보인다.
  - 이는 setTemperature, setScale 2개의 state를 바꾸기 위해 일부러 함수를 사용한 것
  - 만약 변경할 state가 1개 라면 바로 setState를 적용해도 된다.
<br>

**변경 전**
```javascript
...
const handleFahrenheitChange = (temperature) => {
  setTemperature(temperature);
  setScale('f');
}
...

<TemperatureInput
    scale='f'
    temperature={fahrenheit}
    onTemperatureChange={handleFahrenheitChange}/>
...
```
<br>
<br>

**변경 후**
```javascript
...
const handleFahrenheitChange = (temperature) => {
  setTemperature(temperature);
  setScale('f');
}
...

<TemperatureInput
    scale='f'
    temperature={fahrenheit}
    onTemperatureChange={setTemperature}/>
...
```
<br>
<hr>
<br>

**Reference**<br>

[Inje Lee : 처음 만난 리액트(React)](https://www.inflearn.com/course/%EC%B2%98%EC%9D%8C-%EB%A7%8C%EB%82%9C-%EB%A6%AC%EC%95%A1%ED%8A%B8/dashboard)
