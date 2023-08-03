# Jest란?
 - 페이스북에서 만든 자바스크립트 테스팅 라이브러리
 
 - Jest 하나만으로 Test Runner, Test Mathcher, Test Mock 프레임워크까지 제공
   - Test Runner : 테스트 구조에 맞게 테스트 메소드들을 실행하고 결과를 표시
   
   - Test Mathcher : Jest에서 테스트를 위한 함수 ( 값의 비교, Equal, not 등 연산과 관련)
   
<br>
   
![image](https://github.com/bjsystems/rnd/assets/121341413/2455f2ea-01a7-4f89-aa7a-1722d2dc16c8)

<br>

![image](https://github.com/bjsystems/rnd/assets/121341413/25c88eb1-5e3a-4f89-a4a6-aa629397da72)

<br>

![image](https://github.com/bjsystems/rnd/assets/121341413/f3ac2776-5836-4d14-9c76-e995e45ba33b)


 - Mocking : 단위테스트를 작성할 때, 해당 코드가 의존하는 부분을 가짜(mock)로 대체하는 기법 (mock = 모조품)
   즉 테스트하고자 하는 코드가 의존하는 function이나 class에 대해 모조품을 만들어 '**일단'** 돌아가게 하는 것
   - why? : 테스트 하고자하는 기능이 다른 기능과 엮어있는 경우 정확한 테스트를 하기 힘들기 때문

<br>

![image](https://github.com/bjsystems/rnd/assets/121341413/477e2755-3eb8-4a14-9266-aa1182f6abaa)
 - 기존의 데이터베이스 저장 메소드를 mock 함수로 만든다. 
 - controller 로직에 집중을 위해 DB는 "대충 이런 값을 반환한다고 치자"라는 개념


<br>

 - Test Mock : 가짜 DB를 사용하여 controller 로직을 실행시키는 개념

<br>

![image](https://github.com/bjsystems/rnd/assets/121341413/ffda3984-e6c3-41ee-b30b-58fe6c1255cd)
