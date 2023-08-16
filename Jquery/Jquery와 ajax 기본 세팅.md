## Jquery란?
- 제이쿼리(jQuery)는 오픈 소스 기반의 자바스크립트 라이브러리이다.

- 익스플로러 시절 강자로 군림하였으나, 근래에는 더욱 강력한 라이브러리의 등장으로 인해<br>
사장되어 가고 있는 기술이기도 하다. 그러나 여전히 쓰이고는 있다.

```javascript
$(document).ready(function() { // jquery 시작 문법
	  loadList();
  });
  
  function loadList() {
	  $.ajax({ // ajax 시작 문법
		 url: 'boardList.do',
		 type: 'get',
		 dataType: 'json',
		 success: makeView, // 콜백함수
		 error: function(){ alert("error"); }
	  });
  };
  
  function makeView(res) { // 여기서 success를 통해 들어온 인자값을 받는다.
	  alert(res);
  }
```