## 리액트에서 class가 아닌 className을 쓰는 이유
- 리액트에서 class를 사용할 때 className을 쓰는 경우을 봤을 것이다.

- 아래와 같이 일반 html과 리액트는 class를 주는 방식이 다르다.

**html**
- html에서 이것은 attribute 이름으로써 class를 사용하는게 일반적이다.

```javascript
<h1 class="big">Hey</h1>
```
<br>

**React**
-JSX에서 class 대신 className을 사용해야만 한다.

```javascript
<h1 className="big">Hey</h1>
```
<br>
<hr>
<br>

### ✔ 이유
- 간단하게 class는 이미 javascript에서 예약되어 있는 예약어 이기 때문이다.

- 그러므로 class를 쓰지 않고 className을 써야한다.

- className으로 쓰더라도 HTML에서 class로 렌더링이 되는 것이다.
<br>
<hr>
<br>

**Reference**<br>

[by 고코더 : React - 리액트에서는 왜 className으로 사용할까?](https://gocoder.tistory.com/2556)<br>
[lilly.log : class vs. className](https://velog.io/@lillynextdoor/class-vs.-className)
