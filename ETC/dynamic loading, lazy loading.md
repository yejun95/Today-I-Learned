## dynamic loading과 lazy loading
- loading : 데이터를 메모리에 옮기는 것

- 프로그램을 실행시키면, .exe에 있는 파일이 메모리에 올라가야 실행이 되는 것과 같이 데이터를 메모리에 옮기는 것을 로딩 즉<br>
메모리에 적재 한다고 한다.
<br>

### ✔ dynamic loading
- 프로세스가 시작될 때 그 프로세스의 주소 공간 전체를 메모리에 올려놓는 것이 아니라,<br>
메모리를 좀 더 효율적으로 사용하기 위해 필요한 루틴이 호출될 때 해당 루틴을 메모리에 적재하는 방식.

- 즉, 필요한 시점에만 올리니까 메모리를 더 효율적으로 쓰이는게 가능

-  필요할 때만 적재되서 코드 양이 많을 때 자주 호출되지 않는 루틴(에러처리 루틴)에 효율적

-  OS의 특별한 자원을 필요로 하지 않고 프로그래머의 재량에 따라 구현이 가능  

-  그러나 이는 옛날에 메모리가 부족했을때 사용된 방법이고, Virtual Memory Management가 나오고 나서<br>
더 이상 필요하지 않게 되었다.<br>
[Vertual Memory Management]() 
<br>
<hr>
<br>

### ✔ lazy loading
- 중요도가 떨어지거나 당장 화면에 보이지 않는 요소들의 로딩을 우선적으로 시행하지 않으면서<br>
웹페이지 로딩 퍼포먼스를 최적화하는 기술을 의미

- 최초 페이지의 로딩 시간 개선 및 최초 데이터 전달 양 감소에 목적을 둔다.

- 기존 방식은 웹페이지 로드 시 전체 데이터를 한 번에 받아오기 때문에 로딩 시간이 길어졌었다.

- 주로 이미지 or 영상 등에 적용되어, 클라이언트가 원할 때 데이터를 로드한다.

- ex) 무한 스크롤, placeholder, 
<br>
<hr>
<br>

**Reference**<br>

[양햄찌 : Dynamic Loading 동적 적재](https://jhnyang.tistory.com/entry/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9CDynamic-Loading-%EB%8F%99%EC%A0%81%EC%A0%81%EC%9E%AC-Overlays-%EC%98%A4%EB%B2%84%EB%A0%88%EC%9D%B4-paging-VMM%EA%B3%BC-%EC%B0%A8%EC%9D%B4%EC%A0%90)<br>
[vagabondms : Lazy-loading이란 무엇인가.](https://velog.io/@vagabondms/%EA%B8%B0%EC%88%A0-%EC%8A%A4%ED%84%B0%EB%94%94-Lazy-loading%EC%9D%B4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80)<br>
[Hello Inyong : 웹 성능 최적화를 위한 Image Lazy Loading 기법](https://helloinyong.tistory.com/297)

