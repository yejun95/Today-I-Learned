## Each문이란?
- 프로그래밍에서 자주쓰는 for, foreach를 Jquery에서 쓸 수 있는 방법이다.

- 일반 for문보다 가독성이 좋지만 리턴값을 받지 못한다.
<br>
<hr>
<br>

### ✔ 사용 예제
- index : 순번

- el : 데이터

```javascript
var arr= [ 
  {name : '알리송', backnumber : '1'},
  {name : '반다이크', backnumber : '4'}
];

$.Each (arr, function (index, el) {
  console.log('element', index, el);
  console.log(el.name);
  console.log(el.backnumber);
  console.log(el.name + el.backnumber);
});
```
<br>

- 배열 뿐만 아니라 태그도 each문으로 반복시킬 수 있다.

```javascript
<ul class="soccerList"> 
	<li>리버풀</li>
	<li>레알마드리드</li> 
	<li>AS로마</li> 
</ul>

<script type="text/javascript">
$('.soccerList li').each(function (index, el) {
     console.log(el);
});	
</script>


// 결과
// <li>리버풀</li>
// <li>레알마드리드</li>
// <li>AS로마</li>
```
<br>
<hr>
<br>

**Reference**<br>

[인사이드아웃 : 제이쿼리 each, foreach, for](https://lnsideout.tistory.com/entry/jQuery-%EC%A0%9C%EC%9D%B4%EC%BF%BC%EB%A6%AC-each-foreach-for%EB%B0%98%EB%B3%B5%EB%AC%B8-%EC%98%88%EC%A0%9C)<br>
[jaiyah : jQuery의 each() 메서드 알아보기](https://webclub.tistory.com/455)
