# filter함수란?
- filter 함수는 명칭과 같이 callbackFunction의 조건에 해당하는 모든 요소가 있는 배열을
새로 생성하는 기능을 합니다.
- filter 함수의 구문은 아래와 같다.
```javascript
const newArray = arr.filter(callbackFunction(element, index, array), thisArg);
```
- filter 함수의 매개변수는 callbackFunction 과 thisArg 입니다.
- callbackFunction에는 3개의 매개변수를 사용할 수 있습니다.
  - element : 요소값
  - index : 요소의 인덱스
  - array : 사용되는 배열 객체
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
