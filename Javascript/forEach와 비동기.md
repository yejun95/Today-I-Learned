## forEach와 비동기
- forEach()는 동기적으로 동작하기 때문에, promise를 기다려주지 않는다.

- 그렇기 때문에 async/await를 걸어봐도 전혀 효과가 없다.
<br>
<hr>
<br>

### ✔ 해결법
- `for..of()` 사용

- `map()` 사용
<br>
<hr>
<br>

**Reference**<br>

[Rki0.log : async/await와 forEach(), map()](https://velog.io/@rkio/Javascript-asyncawait-%EC%99%80-forEach-map)
