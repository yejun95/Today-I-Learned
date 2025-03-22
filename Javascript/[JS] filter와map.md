# filter함수란?
- 주어진 함수의 <span style="color:yellow">테스트를 통과하는 모든 요소를 모아 새로운 배열로 반환</span>
- filter 함수의 구문은 아래와 같다.
```javascript
const newArray = arr.filter(callbackFunction(element, index, array), thisArg);
```
- filter 함수의 매개변수는 callbackFunction 과 thisArg 입니다.
- callbackFunction에는 3개의 매개변수를 사용할 수 있습니다.
  - element(Optional) : 요소값
  - index(Optional) : 요소의 인덱스
  - array(Optional) : 사용되는 배열 객체
- 그리고 thisArg 는 filter에서 사용될 this 값입니다. 선택적으로 사용되며 사용하지 않을 경우 undefined 전달 됩니다.

💡 **예제**
```javascript
const arr = [1,2,3,4,5];
    const newArray = arr.filter(function(element,index,array) {
        console.log(element);
        console.log(index);
        console.log(array);
    });
```
**실행 결과**

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/fc27524b-92c9-45e8-b2af-fb497c57b86c)

- 위의 설명과 같이 element 에는 현재 순회하는 배열의 인자값, index에는 그 인자값의 인덱스
array에는 현재 배열이 로그창에 찍히게 됩니다.

- JSON 객체에서도 필터 함수가 가능하게 되는데, 아래 예제를 확인하자

💡 **예제**
```javascript
 let testJson = [{name : "이건", salary : 50000000},
                            {name : "홍길동", salary : 1000000},
                            {name : "임신구", salary : 3000000},
                            {name : "이승룡", salary : 2000000}];
            testJson = JSON.stringify(testJson); // JSON 형태로 변환
            const newJson = JSON.parse(testJson).filter(function(element){ // JSON을 자바스크립트 객체로 변환
                console.log(element);
                return element.name == "이건";
            });
            console.log("newObj");
            console.log(newJson);
```
**실행 결과**

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/8be7582f-36a3-44de-905a-8f93cc2acbda)

- element에는 jsonArray의 jsonObject들이 넘어오게 되고, 그중에 name이 "이건" 인 Object를 찾아 리턴합니다.

<hr><br>

# map함수란?
- callback 함수를 각각의 요소에 대해 한번씩 순서대로 불러 그 함수의 반환값으로 새로운 배열을 생성
- 각 요소는 순서대로 함수를 타고, 함수에 정의된 새로운 형태의 배열로 반환되는 것이다.
- map함수의 구문은 아래와 같다.
```javascript
array.map(callBackFunction(element, index, array), thisArg);
```
- map 함수의 매개변수는 callbackFunction 과 thisArg 입니다.
- callBackFunction에는 3개의 매개변수를 사용할 수 있습니다.
  - element(Optional) : 요소값
  - index(Optional) : 요소의 인덱스
  - array(Optional) : 사용되는 배열 객체
- 그리고 thisArg 는 map에서 사용될 this 값입니다. 선택적으로 사용되며 사용하지 않을 경우 undefined 전달 됩니다.

💡 **예제**
```javascript
const arr = [1,2,3,4,5];
        const newArray = arr.map(function(element) {
            return element <= 3;
        });
        console.log(newArray)
```
- 앞선 filter함수와 똑같은 예제이지만 다른 결과를 출력한다.
- map이 산술 연산이기 때문에 모든 것에 대한 새로운 배열 생성

**실행 결과**

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/8c578770-4a32-4225-b216-9e3ac7d6392a)

<hr><br>

# filter와 map의 결정적 차이
```javascript
var testArray = [0,1,2,3,4,5];

testArray.filter(function(c){ return c <= 2; }); // [0, 1, 2]

testArray.map(function(c){ return c <= 2 }); // [true, true, true, false, false, false]

testArray.map(function(c){
    if (c <= 2)
        return c;
}) // [0, 1, 2, undefined, undefined, undefined] 빈값을 제거하기위해선 아이러니하게도 filter을 써야한다.
```

- 왜 filter와 map은 똑같은 코드임에 불구하고 결과값이 차이가 나는걸까?
  - 그것은 콜백함수의 역할이 다르기 때문이다.
- map의 콜백함수는 산술된 인자를 받아 배열을 만드는 역할을 한다.
  - 그래서 c <= 2를 받으면 그 산술 결과인 불리언값을 리턴해 배열을 만드는 것이다.
- filter의 콜백 함수는 리턴값의 불리언이 true인 값만 가지고 배열을 만드는 역할은 한다.
  - 그래서 c <= 2에서 c값에 따라 저 조건식이 true면, c를 그대로 저장하는 것이다.

**이번엔 반대의 경우를 보자. 논리 연산이 아닌 산술 연산을 리턴값으로 주면 어떻게 될까?**

```javascript
var testArray = [0,1,2,3,4,5];

testArray.filter(function(c){ return c * 2; }); // [1, 2, 3, 4, 5]

testArray.map(function(c){ return c * 2 }); // [0, 2, 4, 6, 8, 10]
```

map은 산술된 값을 리턴, 즉 c * 2를 산술해서 배열에 넣어 만드니까 원하는 답을 얻을수 있다.

하지만, filter은 c * 2를 산술이 아닌 논리로 본다. c * 2는 뭘해도 참이다.

딱 한가지만 제외하고 말이다.

바로 c에 0값이 들어가면 0이 되니까, 0은 fasle.

그래서 위 filter()의 결과 배열에 '0' 원소가 빠져있는 이유이다!

[참고 : 자바스크립트 map 과 filter 차이](https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-map-%EA%B3%BC-filter-%EC%B0%A8%EC%9D%B4)
