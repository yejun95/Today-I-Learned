## for in과 for of의 차이
- javascript를 사용하다 보면 객체를 순환하여 데이터를 가져 올 때 기존 for문 대신<br>
`const item in obj`, `const item of obj`와 같은 형태로 사용할 때가 있다.

- in과 of의 차이에 대해서 알아보고자 한다.
<br>
<hr>
<br>

### ✔ for in
- 객체를 순환하여 가져온다.

- 주로 key값이나 index를 가져올 때 사용한다. (배열의 경우 index를 리턴해준다)

- value값에는 직접 접근 불가

- 순서가 보장되지 않아 순서가 중요한 경우 사용하지 않는 것이 좋다.

- length 연산자 사용 불가능
<br>

**예제 소스**
```javascript
// ex) for in

const obj = {
  a: 1,
  b: 2,
  c: 3
};

for(const item in obj) {
	console.log("for in : " + item);
};
```
<br>

**결과값**
<br>
![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/5df5500e-4512-43d8-bc92-31349279a618)
<br>
- 객체의 key 값이 반환
<br>
<hr>
<br>

### ✔ for of
- 배열을 순환하여 가져온다.
  - 반복 가능한 객체(iterable)를 순회할 수 있도록 해줌

- 배열의 값(value)를 가져올 때 쓴다.

- Array, Map, Set, arguments 등이 해당됨 (Object는 해당 X) 
<br>

**예제 소스**
```javascript
const arr = ['a','b','c'];

for(const item of arr) {
	console.log("for of : " + item);
}
```
<br>

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/68e95a64-b2eb-4735-8996-b2816c2c9f15)
<br>

- 배열의 value값이 전부 출력된다.

- for in과 같은 결과를 출력한 것처럼 보이지만 for in은 객체의 key를 for of는 배열의 값을 리턴한 것이다.
<br>

**Type별 예제**
```javascript
// Array 
for (const val of ['a', 'b', 'c']) {
	console.log(val); // 'a','b','c' 
} 

// String 
for (const val of 'abc') { 
	console.log(val); // 'a','b','c' 
} 

// Object 
for ( let val of {1 :'a', 2 :'b', 3 :'c'} ) {
	console.log(val); // TypeError: object is not iterable 
}
```
<br>

💡 **한마디로 가장 큰 차이점은 key를 순회하냐 value를 순회하냐 이다.**
<br>

[reduce로 값은 배열끼리 객체로 묶고 for in, for of의 이중배열을 통한 특정값 뽑아내기 예제]()

<br>
<hr>
<br>

**Reference**<br>

[Carl's Tech Blog : for in vs for of 차이](https://wotres.tistory.com/entry/javascript-for-in-vs-for-of-%EC%B0%A8%EC%9D%B4)<br>
[joooing : for in문, for of문의 차이점](https://joooing.tistory.com/entry/Iteration2-for-in%EB%AC%B8-for-of%EB%AC%B8)
