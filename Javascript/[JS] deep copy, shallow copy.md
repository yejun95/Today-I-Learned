- 깊은 복사(Deep Copy)는 '실제 값'을 새로운 메모리 공간에 복사하는 것을 의미하며,

- 얕은 복사(Shallow Copy)는 '주소 값'을 복사한다는 의미입니다.

- 모든 데이터 타입은 값 타입(value type) 또는 참조 타입(reference type)을 가진다.

- 값 타입(Value type) : 각각의 고유의 메모리를 소유한다. 원시 타입(Primitive Type)과 비슷하다.
  - 아래의 6가지 데이터 타입을 자바스크립트에서 값 타입(Value Type)이라고 한다.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/161fc9d6-5f16-4052-bec4-f73bffa4a7e9)


- 참조 타입(Reference type) : 생성된 인스턴스들은 주소값을 공유한다. 스위프트에서 class가 해당 타입에 속한다.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/feeac5dc-b30a-414e-812a-1ad360e891a2)


**값 타입과 참조 타입이냐에 따라 달라지니 타입을 잘 봐야한다.**
<br>
<hr>
<br>

## 얕은 복사 Shallow Copy
### ✔ 값 타입
- 애초에 고유의 메모리를 소유하기 때문에 기본적으로 깊은 복사가 일어난다.
<br>
<br>

### ✔ 참조타입
```javascript
const obj1 = { a: 1, b: 2};
const obj2 = obj1;

console.log( obj1 === obj2 ); // true
```

- 위의 예시처럼 객체를 직접 대입하는 경우 참조에 의한 할당이 이루어지므로 둘은 같은 데이터(주소)를 가지고 있다.
<br>

```javascript
const obj1 = { a:1, b:2 };
const obj2 = obj1;

obj2.a = 100;

console.log( obj1.a ); // 100
```

- 위 두 객체는 같은 데이터(주소)를 가지고 있고, 그래서 같은 주소를 참조하고 있다.

- 때문에 obj2의 property를 수정하고, obj1를 출력해도 obj2 값과 동일하다.
<br>
<hr>
<br>

## 깊은 복사 (deep copy)
### ✔ 값 타입
```javascript
let string1 = "감자";
let string2 = string1;
string2 = '사과';

console.log("string1", string1); // 감자
console.log("string2", string2); // 사과
```

- string2에 사과를 대입하였지만 string1 값은 변하지 않았다.

- 값 타입으로 깊은 복사가 기본으로 일어나기 때문에 서로 고유한 메모리를 보유하고 있는 것이다.
<br>
<br>

### ✔ 참조 타입
- 참조 타입은 기본적으로 주소값을 참조할 수 있기 때문에 얕은 복사가 일어난다.

- 이를 깊은 복사로 하기 위해서는 강제로 새 객체를 만들어줘야 한다.
  - `JSON.parse(JSON.stringify())`
<br>

```javascript
const obj1 = {a:1, b:2};
const obj2 = JSON.parse(JSON.stringify(obj1))
obj2.a = 100

console.log("obj1", obj1) // {a:1, b:2}
console.log("obj2", obj2) // {a:100, b:2}
```

- 깊은 복사가 일어나므로 `obj2.a`의 값을 변경하여도 `obj1`에 영향을 주지 않는다.
<br>
<hr>
<br>

**Reference**<br>

[HANAMON : JavaScript 얕은 복사(shallow copy) vs 깊은 복사(deep copy)](https://hanamon.kr/javascript-shallow-copy-deep-copy/)<br>
[ellyheetov.devlog : 깊은 복사 VS 얕은 복사](https://velog.io/@ellyheetov/Shallow-Copy-VS-Deep-Copy)<br>
[Minsu's Dev Log : 값 타입(Value Type)과 참조 타입(Reference Type))](https://alstn2468.github.io/Javascript/2020-05-08-ValueTypeReferenceType)
