## JSON.parse와 JSON.stringify의 차이

**JSON이란?**
- JavaScript Object Notation을 의미하며 경량화한 텍스트기반 데이터 교환 방식을 의미한다.

- 즉, 문자 기반의 데이터 포맷이다.

- 메서드는 담을 수 없고, 프로퍼티만 담을 수 있다.

- 객체, 배열, 숫자(NaN, Infinity는 제외), 문자열, bool, null을 직렬화 할 때 사용한다.
  - 직렬화 : 외부에서 사용할 수 있도록 데이터를 변환(객체를 문자열로)하는 것
  - undefined도 JSON에서는 지원하지 않는 형식이므로 직렬화되지 않는다.
<br>
<hr>
<br>

### ✔ JSON.parse()
- 전달받은 문자열을 자바스크립트 객체로 변환한다.

**예제 소스**
```javascript
{
  "name": "홍길동",
  "age": 25,
  "married": false,
  "family": { "father": "홍판서", "mother": "춘섬" },
  "hobbies": ["독서", "도술"],
  "jobs": null
}
```
<br>

- 상단의 JSON 문자열을 변수에 담아 JSON.parse()를 진행하면 아래와 같은 결과를 얻을 수 있다.

```javascript
const str = `{
  "name": "홍길동",
  "age": 25,
  "married": false,
  "family": { "father": "홍판서", "mother": "춘섬" },
  "hobbies": ["독서", "도술"],
  "jobs": null
}`;

const obj = JSON.parse(str);

console.log(obj);
```

```javascript
{
    name: "홍길동",
    age: 25,
    married: false,
    family: {
        father: "홍판서",
        mother: "춘섬"
    },
    hobbies: [
        "독서",
        "도술"
    ],
    jobs: null
}
```
<br>

- 제일 처음 JSON 문자열 에서는 쌍따옴표로 모든 key 값을 감싸줬지만 자바스크립트 객체에서는 그럴 필요가 없다.
  - 특수 문자나 영어 외의 언어와 같이 키로 허용되지 않는 문자를 키로 사용해야하는 경우에는 쌍따옴표를 사용
 
- 또한 이렇게 변환된 자바스크립트 객체는 `.`, `[]` 기호를 사용하여 접근이 가능하다.
```javascript
> obj.name
< '홍길동'
> obj.age
< 25
> obj.married
< false
> obj.family
< {father: '홍판서', mother: '춘섬'}
> obj.family.mother
< '춘섬'
> obj.hobbies
< ['독서', '도술']
> obj.hobbies[1]
< '도술'
> obj.jobs
< null
```
<br>

- 이렇게 외부에서 문자열의 형태로 주어진 데이터를 해당 언어에서 다루기 용이하도록 내장 데이터 타입으로<br>
 변환하는 과정을 CS(Computer Science)에서는 소위 역직렬화(deserialization)이라고 부른다.

- 대표적인 사례로 클라이언트에서 JSON 포맷으로 데이터를 보내면,<br>
 서버에서 우선 JavaScript 객체로 변환한 후에 데이터를 처리하게 된다.
<br>
<hr>
<br>

### ✔ JSON.stringify()
- javaScript 객체를 인자로 받아 JSON 문자열로 변환한다.

- 위에서 parse() 메서드의 호출 결과로 얻은 JavaScript 객체를 obj이라는 변수에 저장하여 예제 소스 진행

```javascript
const obj = {
  name: "홍길동",
  age: 25,
  married: false,
  family: {
    father: "홍판서",
    mother: "춘섬",
  },
  hobbies: ["독서", "도술"],
  jobs: null,
};

const str = JSON.stringify(obj);

console.log(str);
```

```javascript
'{"name":"홍길동","age":25,"married":false,"family":{"father":"홍판서","mother":"춘섬"},"hobbies":["독서","도술"],"jobs":null}'
```
<br>

- javascript 객체가 JSON 문자열의 형태로 바뀐 것을 볼 수 있다. (key, value에 모두 쌍따옴표가 붙음)

- 그러나 양이 많아지면 한줄로 출력이 되어 알아보기 힘든데, JSON.stringify의 3번째 인자를 활용하여 공백 주기가 가능

**JSON.stringify의 공백 주기**
```javascript
const str2 = JSON.stringify(obj, null, 2);
console.log(str2);
```

```javascript
{
  "name": "홍길동",
  "age": 25,
  "married": false,
  "family": {
    "father": "홍판서",
    "mother": "춘섬"
  },
  "hobbies": [
    "독서",
    "도술"
  ],
  "jobs": null
}
```
<br>

- 당연하겠지만, 더 이상 javascript 객체에서의 접근 방식인 `.`, `[]`는 사용할 수 없다.

- 이렇게 특정 언어의 내장 타입의 데이터를 외부에 전송하기 용이하도록 문자열로 변환하는 과정을<br>
 CS(Computer Science)에서는 소위 직렬화(serialization)이라고 부른다.

- 대표적인 사례로 서버에서 클라이언트의 요청을 처리 후에 JavaScript 객체 형태의 데이터를<br>
 JSON 형태의 문자열로 변환하여 응답하는 것을 들 수 있습니다.
<br>
<hr>
<br>

**Reference**<br>

[reasonz : JSON 개념과 JSON.parse, JSON.stringify 메서드](https://velog.io/@reasonz/JSON-%EA%B0%9C%EB%85%90%EA%B3%BC-JSON.parse-JSON.stringify-%EB%A9%94%EC%84%9C%EB%93%9C)<br>
[DaleSeo : JSON.parse()와 JSON.stringify()](https://www.daleseo.com/js-json/)<br>
  
