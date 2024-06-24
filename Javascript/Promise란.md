# Promise란 무엇인가?
- 자바스크립트 비동기 처리에 사용되는 객체
  - 비동기 : 특정 코드의 실행이 완료될 때까지 기다리지 않고 다음 코드를 먼저 수행하는 자바스크립트의 특성
<br>
<hr>
<br>

## ✔️ 필요한 이유
- 프로미스는 주로 서버에서 데이터를 받아올 때 사용한다.

- 이 때, 데이터를 다 받아오기도 전에 화면에 데이터를 표시하려고 하면 오류가 발생하거나 빈 화면이 뜰 것이다.

- 이 같은 문제점을 해결하기 위해 사용하는 객체이다.
<br>

### axios 테스트
- 앞서 데이터를 다 받아오기 전에 화면에 데이터를 표시하려하면 오류가 발생한다고 했다.

- 그에 대한 테스트 검증 진행

- axios 예제로 비동기 처리를 하지 않고 콘솔을 통해 함수를 실행시켜 json 데이터를 가져와본다.
```javascript
const axios = require('axios');

function getData() {
	axios.get('http://echo.jsontest.com/key/value/one/two')
    .then(res => {
      console.log(res.data)
    })
    .catch(err => {
      console.log(err)
    })
}

console.log(getData());
```
<br>

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/78fdecab-b13e-474d-b74c-bf9eb8b3fbd3)
> json 데이터를 제대로 가져오지 못하고, 이후 axios 코드 내부의 then으로 인해 정상 데이터 출력
<br>

- 이렇듯 자바스크립트의 비동기 특성을 해결하고자 사용하는 것이 Promise이다.

- Promise를 사용하면 API에 호출에 대한 데이터를 정상적으로 받아올 수 있다.
<br>
<hr>
<br>

## ✔️ 사용법
```javascript
const axios = require('axios');

function getData() {
  // new Promise() 추가
  return new Promise(function(resolve, reject) {
    axios.get('url 주소')
      .then(res => {
        resolve(res.data)
      })
  });
}

// getData()의 실행이 끝나면 호출되는 then()
getData().then(function(axiosData) {
  // resolve()의 결과 값이 여기로 전달됨
  console.log(axiosData); // axios.get()의 response 값이 axiosData 전달됨
});
```
> getData 함수안에 Promise를 사용하고 이에 대한 결과를 then으로 받는다.
<br>

- 이전에 사용했던 axios 예제에 위와 같은 사용법을 통해서 Promise로 변경시켜보자.

```javascript
const axios = require('axios');

function getData() {
  return new Promise(function(resolve, reject) {
    axios.get('http://echo.jsontest.com/key/value/one/two')
      .then(res => {
        resolve(res.data)
      })
  });
}

getData().then(function(axiosData) {
  console.log(axiosData);
});
```
<br>

- 콘솔 결과

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/8aba3349-d423-46d2-a462-5aa4c3809097)
> 이전과는 다르게 undefined가 나오지 않고 바로 api 데이터가 출력됨
<br>

- 이처럼 Promise는 주로 API 데이터를 가져올 때, 자바스크립트의 비동기 특성을 해결하기 위해 사용한다.
<br>
<hr>
<br>

