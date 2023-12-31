## interpreter
- 컴파일러 방식과는 다르게 고급어 소스코드를 직접 실행하는 프로그램이나 환경을 말한다.
  - 컴파일러 방식은 고급어 -> 저급어로 번역하는 과정을 거쳐야한다.
<br>

- 보통 한번에 한줄 단위로 실행한다.
  - 전체를 한번에 번역하는 컴파일러 방식과 다르게 한줄씩 실행한다.
<br>

- 성능(특히 속도)면에서 컴파일러 방식보다 느리다.
  - 컴파일러 방식은 전체를 한번에 번역해서 실행하는데 반면,<br>
  인터프리터 방식은 한줄씩 실행되기 때문이다.

- Javascript나 Python이 여기에 해당된다.
<br>
<hr>
<br>

### ✔ 컴파일러 방식과 비교
**유연성**
- 컴파일러 방식 : 낮다.  ->  코드 한줄만 바뀌어도 전체를 다시 번역해는 과정을 거치기 때문이다.
  - 한줄이라도 오류  ->  실행 불가
<br>

- 인터프리터 방식 : 높다.  ->  코드 한줄만 바뀌어도 어차피 소스를 한줄씩 실행하므로 무리가 없다.
  - 소스에 오류가 있어도 에러 전까지 실행
<br>
<br>

**성능**
- 컴파일러 방식 : 빠르다.  ->  전체를 한번에 번역, 그러나 번역 속도는 인터프리터보다 느리다.
  - 실행 파일을 만들기 때문에 메모리를 사용한다.
<br>

- 인터프리터 방식 : 느리다.  ->  소스 코드를 한줄씩 실행하기 때문이다, 그러나 번역 속도는 컴파일러보다 빠르다.
  - 고급어를 바로 번역하기 때문에 실행파일이 없어 메모리를 사용하지 않는다.
<br>
<hr>
<br>

**Reference**<br>

[널널한 개발자 : 넓고 얕게 배워서 컴공 전공자 되기](https://www.inflearn.com/course/%EB%84%93%EA%B3%A0%EC%96%95%EA%B2%8C-%EC%BB%B4%EA%B3%B5-%EC%A0%84%EA%B3%B5%EC%9E%90/dashboard)
