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
import axios from 'axios'

function getData() {
  axios.get('http://echo.jsontest.com/key/value/one/two')
    .then(response => {
      return response.data
    })
    .catch(error => {
      console.log (error)
    });
}

function run() {
  const data = getData();
  console.log(data)
}

run();
```
<br>

![image](https://github.com/bjsystems/rnd/assets/121341413/a98c9a69-e63b-4626-a888-acc08bbb696a)
> data가 넘어오기 전에 출력했기 때문에 `undefined` 발생
<br>

- 이렇듯 자바스크립트의 비동기 특성을 해결하고자 사용하는 것이 Promise이다.

- Promise를 사용하면 API에 호출에 대한 데이터를 정상적으로 받아올 수 있다.
<br>
<hr>
<br>

## ✔️ 사용법
```javascript
import axios from 'axios'

function getData() {
  return new Promise(function(resolve, reject) {
    axios.get('http://echo.jsontest.com/key/value/one/two')
      .then(response => {
        resolve(response.data);
      })
      .catch(error => {
        reject(error);
      });
  });
}

function run() {
  getData()
  .then(function(data) {
    console.log(data);
  })
  .catch(function(error) {
    console.error(error);
  });
}

run();
```
> getData 함수안에 Promise를 사용하고 이에 대한 결과를 then으로 받는다.
<br>

- 콘솔 결과

![image](https://github.com/bjsystems/rnd/assets/121341413/cadfe308-c9b6-42ca-b5ce-d12773bf97d4)
> 이전과는 다르게 undefined가 나오지 않고 바로 api 데이터가 출력됨
<br>

- 이처럼 Promise는 주로 API 데이터를 가져올 때, 자바스크립트의 비동기 특성을 해결하기 위해 사용한다.
<br>
<hr>
<br>

## ✔️ Promise의 상태(state)
- 프로미스는 3가지의 상태를 가진다.

- 여기서 말하는 상태란 프로미스의 처리 과정을 의미

- `new Promise()`로 프로미스를 생성하고 종료될 때 까지의 3가지 상태이다.
  - Pending(대기) : 비동기 처리 로직이 아직 완료되지 않은 상태
  - Fulfilled(이행) : 비동기 처리가 완료되어 프로미스가 결과 값을 반환해준 상태
  - Rejected(실패) : 비동기 처리가 실패하거나 오류가 발생한 상태
<br>

- 이를 코드로 확인해보자.
<br>

### Pending
- 먼저 아래와 같이 new Promise() 메서드를 호출하면 대기(Pending) 상태가 된다.
```javascript
new Promise();
```
<br>

- `new Promise()` 메서드를 호출할 때 콜백 함수를 선언할 수 있고, 콜백 함수의 인자는 resolve, reject이다.

```javascript
new Promise(function(resolve, reject) {
  // ...
});
```
<br>
<br>

### Fulfilled(이행)
- 콜백 함수의 인자 resolve를 아래와 같이 실행하면 이행(Fulfilled) 상태가 된다.

```
new Promise(function(resolve, reject) {
  resolve();
});
```
<br>

- 그리고 이행 상태가 되면 아래와 같이 then()을 이용하여 처리 결과 값을 받을 수 있다.

```javascript
function getData() {
  return new Promise(function(resolve, reject) {
    var data = 100;
    resolve(data);
  });
}

// resolve()의 결과 값 data를 resolvedData로 받음
getData().then(function(resolvedData) {
  console.log(resolvedData); // 100
});
```
<br>
<br>

### Rejected(실패)
- `new Promise()`로 프로미스 객체를 생성하면 콜백 함수 인자로 resolve와 reject를 사용할 수 있다고 하였다.

- 여기서 reject를 아래와 같이 호출하면 실패(Rejected) 상태가 된다.

```javascript
new Promise(function(resolve, reject) {
  reject();
});
```
<br>

- 그리고, 실패 상태가 되면 실패한 이유(실패 처리의 결과 값)를 `catch()`로 받을 수 있다.

```javascript
function getData() {
  return new Promise(function(resolve, reject) {
    reject(new Error("Request is failed"));
  });
}

// reject()의 결과 값 Error를 err에 받음
getData().then().catch(function(err) {
  console.log(err); // Error: Request is failed
});
```
<br>
<hr>
<br>

## ✔️ 실습 예제
- 이제 resolve와 reject를 둘다 사용하는 코드를 작성해보자.

- 제일 처음 사용했떤 test API 주소를 그대로 사용하여 json 데이터를 가져온다.

```javascript
const axios = require('axios');

function getData() {
  // new Promise() 추가
  return new Promise(function(resolve, reject) {
    axios.get('http://echo.jsontest.com/key/value/one/two')
      .then(res => {
        resolve(res.data)
      })
      .catch(err => {
        reject(err)
      })
  });
}

getData().then(function(axiosData) {
  console.log(axiosData);
}).catch(function(err) {
  console.error(err)
})
```
<br>

- get 요청에 대해 성공하면 then 구문에서 `axiosData` 콜백으로 데이터를 받는다.

- get 요청에 대해 실패하면 catch 구문에서 ``err` 콜백으로 데이터를 받는다.
<br>
<br>

- get 요청 성공

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/14a2d348-7e99-42b0-a409-8fb6e18779d2)
<br>

- json 데이터 주소를 임의로 변경하여 실패 테스트 진행

```
  cause: Error: getaddrinfo ENOTFOUND echo.jsontest.codm
      at GetAddrInfoReqWrap.onlookup [as oncomplete] (node:dns:107:26) {
    errno: -3008,
    code: 'ENOTFOUND',
    syscall: 'getaddrinfo',
    hostname: 'echo.jsontest.codm'
  }
}
```
> 로그가 많이 찍혔지만 cause 블록만 캡쳐하였음
<br>

- 이처럼 요청 api에 대해 비동기 처리를 하면서 성공, 실패 데이터를 구분할 수 있다.
<br>
<br>

## ✔️ 여러 개의 프로미스 연결하기 (Promise Chaining)
- 프로미스의 특징 중 하나로써, 여러 개의 프로미스를 연결하여 사용할 수 있다.

- 아래와 같이 then 구문을 통해 연결 가능

```javascript
function getData() {
  return new Promise({
    // ...
  });
}

// then() 으로 여러 개의 프로미스를 연결한 형식
getData()
  .then(function(data) {
    // ...
  })
  .then(function() {
    // ...
  })
  .then(function() {
    // ...
  });
```
<br>

- 이전 예제에 then 구문을 추가하여 작성해보자.

```javascript
const axios = require('axios');

function getData() {
  // new Promise() 추가
  return new Promise(function(resolve, reject) {
    axios.get('http://echo.jsontest.com/key/value/one/two')
      .then(res => {
        resolve(res.data)
      })
      .catch(err => {
        reject(err)
      })
  });
}

// getData()의 실행이 끝나면 호출되는 then()
getData()
  .then(function(axiosData) {
    // resolve()의 결과 값이 여기로 전달됨
    console.log(axiosData); // axios.get()의 response 값이 axiosData 전달됨
    return 10
  })
  .then(function(result) {
    console.log(result + 10)
    return result
  })
  .then(function(result) {
    console.log(result + 20)
  })
  .catch(function(err) {
    console.error(err)
  })
```
<br>

- 기존에 리턴받은 json 데이터와 별개로 처음 then 구문이 끝날 때 숫자 10을 리턴해준다.
  - 두 번째 then에서 숫자 10을 result라는 콜백으로 받고 또 10을 더해 리턴한다.
  - 세 번째 then에서 최종적으로 20 + 10을이 되어 30을 리턴한다.
<br>

- 이런식으로 then을 여러 개 사용하여 로직이 성공했을 때, 이어서 이벤트를 실행시킬 수 있다.
  - Ex) api 요청을 통해 then으로 데이터를 받음 -> 또 then을 사용하여 해당 데이터를 이용하여 또 다른 api 요청 등등
<br>
<hr>
<br>

## ✔ waterfall
- 비동기 작업을 순차적으로 수행하는 패턴

- 즉, 각 작업이 완료된 후 다음 작업이 시작

- 종속적인 비동기 작업을 처리할 때 유용

- 기존 예제를 waterfall 형식을 사용하여 데이터 확인 진행

```javascript
import axios from 'axios'

function getData1() {
  return new Promise(function(resolve, reject) {
    axios.get('http://echo.jsontest.com/key/value/one/two')
      .then(response => {
        resolve(response.data);
      })
      .catch(error => {
        reject(error);
      });
  });
}
function getData2(resultFromGetData1) {
  return new Promise(function(resolve, reject) {
    axios.get('http://echo.jsontest.com/key/value/three/four')
      .then(response => {
        resolve({...resultFromGetData1, ...response.data});
      })
      .catch(error => {
        reject(error);
      });
  });
}
function getData3(resultFromGetData2) {
  return new Promise(function(resolve, reject) {
    axios.get('http://echo.jsontest.com/key/value/five/six')
      .then(response => {
        resolve({...resultFromGetData2, ...response.data});
      })
      .catch(error => {
        reject(error);
      });
  });
}

function run() {
  getData1()
    .then(result1 => getData2(result1))
    .then(result2 => getData3(result2))
    .then(result3 => {
      console.log(result3)
    })
    .catch((error) => {
      console.error(error);
    });
}

run();
```
<br>

- 최종 함수 `getData3`에서 모든 데이터를 출력하였음

![image](https://github.com/bjsystems/rnd/assets/121341413/abc153f6-c1b7-4884-a66e-e187d16ceaa1)
<br>

- 어떤 데이터를 받느냐에 따라 다르겠지만, 이처럼 순차적 비동기 호출을 통해 원하는 데이터를
뽑아낼 수 있다.

- 만약 `getData3`함수가 무조건 `getData1`함수의 데이터가 필요하다면 이런식으로 waterfall을 통해
데이터를 맵핑할 수 있을 것이다.
<br>
<br>

- 지금은 waterfall 형식으로 진행을 한 것이고, async 라이브러리를 통해 waterfall 함수 자체를 사용해보자.

- async 라이브러리 설치
  - `npm install async`
<br>

- 사용 방법

```javascript
async.waterfall([
    function task1(callback) {
        console.log('task1 message');
        callback(null, 'result1');
    },
    function task2(arg, callback) {
        console.log('task2 message' + arg);
        callback(null, 'result2-1', 'result2-2');
    },
    function task3(arg1, arg2, callback) {
        console.log('task3 message' + arg1 + arg2);
        callback(null, 'result3');
    }
    ],
    function (err, results) {
        console.log('task finish');
        console.log(results);
    }
);
```
<br>

- 다음 태스크로 전달할 값을 콜백의 파라미터로 넘긴다.

- callback의 첫번째 인수인 null은 에러가 없다는 표시, 만약 해당 위치에 값을 넣으면 해당 값이
에러로 출력된다.
<br>

- 이제 해당 형식을 본 예제에 대입해보자.

```javascript
import async from 'async';
import axios from 'axios';

function getData1(callback) {
  axios.get('http://echo.jsontest.com/key/value/one/two')
    .then(response => {
      callback(null, response.data);
    })
    .catch(error => {
      callback(error);
    });
}

function getData2(prevData, callback) {
  axios.get('http://echo.jsontest.com/key/value/three/four')
    .then(response => {
      callback(null, { ...prevData, ...response.data });
    })
    .catch(error => {
      callback(error);
    });
}

function getData3(prevData, callback) {
  axios.get('http://echo.jsontest.com/key/value/five/six')
    .then(response => {
      callback(null, { ...prevData, ...response.data });
    })
    .catch(error => {
      callback(error);
    });
}

async.waterfall([
  getData1,
  getData2,
  getData3
], (err, result) => {
  if (err) {
    console.error(err);
  } else {
    console.log('All tasks complete', result);
  }
});
```
<br>

![image](https://github.com/bjsystems/rnd/assets/121341413/43603073-4270-4540-a42b-df31ecbaec57)
> 이전 예제와 동일하게 모든 데이터를 출력
<br>

- 이러한 방식을 통해서 순차적으로 비동기 호출을 통해 데이터를 리턴받을 수 있다.
<br>
<hr>
<br>

## ✔ parelell
- 여러 비동기 작업을 동시에 실행

- 모든 작업이 완료된 후에 결과를 처리하는 패턴

- 우선 async 라이브러리를 통해 parelell 함수를 사용하지 않고 Promise 함수인 `Promise.all`을 사용하여 진행

```javascript
import axios from 'axios'

function getData1() {
  return new Promise(function(resolve, reject) {
    axios.get('http://echo.jsontest.com/key/value/one/two')
      .then(response => {
        resolve(response.data);
      })
      .catch(error => {
        reject(error);
      });
  });
}
function getData2() {
  return new Promise(function(resolve, reject) {
    axios.get('http://echo.jsontest.com/key/value/three/four')
      .then(response => {
        resolve(response.data);
      })
      .catch(error => {
        reject(error);
      });
  });
}
function getData3() {
  return new Promise(function(resolve, reject) {
    axios.get('http://echo.jsontest.com/key/value/five/six')
      .then(response => {
        resolve(response.data);
      })
      .catch(error => {
        reject(error);
      });
  });
}

function run() {
  Promise.all([getData1(), getData2(), getData3()])
    .then(results => {
      console.log(results);
    })
    .catch(error => {
      console.error(error);
    });
}

run();
```
<br>

![image](https://github.com/bjsystems/rnd/assets/121341413/1c408d2c-7c68-41f0-a731-104938974e49)
> 모든 데이터가 병렬로 실행됨
<br>

- 하지만 3개 함수 중 1개라도 제대로 실행되지 않는다면 아래와 같이 에러를 리턴한다.

```
....
....

  cause: Error: getaddrinfo ENOTFOUND echo.jsotest.com
      at GetAddrInfoReqWrap.onlookup [as oncomplete] (node:dns:108:26) {
    errno: -3008,
    code: 'ENOTFOUND',
    syscall: 'getaddrinfo',
    hostname: 'echo.jsotest.com'
  }
}
```
> api url을 잘못된 url로 수정하여 실행하였더니, 리턴된 에러
<br>
<br>

- 이제 async 라이브러리의 paralell 함수를 이용하여 코드 리팩토링 진행

- 사용 방법

```javascript
async.parallel([
    function task1(callback) {
        console.log('task1 start');
        setTimeout(function() {
            console.log('task1 end');
            callback(null, 'result1');
        }, 300);
        console.log('task1 message');
    },
    function task2(callback) {
        console.log('task2 start');
        setTimeout(function() {
            console.log('task2 end');
            callback(null, 'result2');
        }, 500);
        console.log('task2 message');
    },
    function task3(callback) {
        console.log('task3 start');
        setTimeout(function() {
            console.log('task3 end');
            callback(null, 'result3');
        }, 200);
        console.log('task3 message');
    }
    ],
    function (err, results) {
        console.log('task finish');
        console.log(results);
        console.timeEnd('PARALLEL_TIMER');
    }
);
```
<br>

- 기존에 waterfall이랑 사용했던 방법과 비슷하다.

- 이제 기존 Promise.all을 사용했던 예제에 paralell 함수를 적용해보자.

```javascript
import async from 'async';
import axios from 'axios';

function getData1(callback) {
  axios.get('http://echo.jsontest.com/key/value/one/two')
    .then(response => {
      callback(null, response.data);
    })
    .catch(error => {
      callback(error);
    });
}

function getData2(callback) {
  axios.get('http://echo.jsontest.com/key/value/three/four')
    .then(response => {
      callback(null, response.data);
    })
    .catch(error => {
      callback(error);
    });
}

function getData3(callback) {
  axios.get('http://echo.jsontest.com/key/value/five/six')
    .then(response => {
      callback(null, response.data);
    })
    .catch(error => {
      callback(error);
    });
}

async.parallel([
  getData1,
  getData2,
  getData3
], (err, result) => {
  if (err) {
    console.error(err);
  } else {
    console.log(result);
  }
});
```
<br>

![image](https://github.com/bjsystems/rnd/assets/121341413/87cbb3b8-5414-4710-9d21-96cd83a95fbf)
> 모든 비동기 api에 대한 데이터가 리턴됨
<br>

- 이처럼 waterfall을 통해 순차적 실행, paralell을 통해 병렬 실행을 통하여 종속적인 데이터 관리가 가능하다.
<br>
<hr>
<br>

**Reference**<br>

[캡틴판교 : 자바스크립트 Promise 쉽게 이해하기](https://joshua1988.github.io/web-development/javascript/promise-for-beginners/)<br>
[캡틴판교 : 자바스크립트 비동기 처리와 콜백 함수](https://joshua1988.github.io/web-development/javascript/javascript-asynchronous-operation/)<br>
[Onigirl : Promise.all()로 비동기 처리를 구현해보자](https://velog.io/@jay2u8809/Promise.all-%EB%A1%9C-%EB%B9%84%EB%8F%99%EA%B8%B0-%EC%B2%98%EB%A6%AC%EB%A5%BC-%EA%B5%AC%ED%98%84%ED%95%B4-%EB%B3%B4%EC%9E%90)<br>
[빨간색소년 : Node의 흐름 제어 (콜백지옥 탈출, Async, Promise)](https://sjh836.tistory.com/83)
