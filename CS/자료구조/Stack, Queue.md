- Stack과 Queue는 선형 자료구조로써 1차원의 형태를 가진다.
<br>

## Stack
![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/b069f2ff-026e-44c4-95fd-286114214572)

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/beb1a704-628d-4a99-842c-6ff0754111cb)


- 선형 자료로 1차원의 형태를 띈다.

- Last in First Out : 후입 선출

- 삽입 연산 : Push

- 삭제 연산 : Pop

- ex) 되돌아가기 (ctrl + z)

- Stack underflow : 비어있는 스택에서 원소를 추출하려고 할 때

- Stack overflow : 스택이 넘치는 경우
<br>
<hr>
<br>

## Queue
![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/dc29b66f-f8f3-4147-8d23-820d28f8317b)

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/2531c40c-4aaa-4209-8247-dd894176d037)


- 선형 자료로 1차원의 형태를 띈다.

- First in First out : 선입 선출

- 삽입 연산 : enQueue

- 삭제 연산 : deQueue

- ex) 은행 업무, 프린터의 인쇄 대기열

- 동기화?
<br>
<br>

**큐 선언**
```
Queue<Integer> q = new LinkedList<>();
```
<br>
<br>

**큐에 값 추가**
```
q.add(x);
q.offer(x);
```
- add()
  - 해당 큐 맨 뒤에 값 삽입
  - 값 추가 성공 시 true 반환
  - 큐가 꽉 찬 경우 IllegalStateException 에러 발생
<br>

- offer()
  - 해당 큐 맨 뒤에 값 삽입
  - 값 추가 성공 시 true 반환
  - 값 추가 실패 시 false 반환
<br>
<br>

**큐에 값 제거**
```
q.remove();
q.poll();
q.clear();
```
- remove()
  - 큐 맨 앞에 있는 값 반환 후 삭제
  - 큐가 비어 있는 경우 NoSuchElementException 에러 발생
<br>

- poll()
  - 큐 맨 앞에 있는 값 반환 후 삭제
  - 큐가 비어있을 경우 null 반환
<br>

- clear()
  - 큐 비우기
<br>
<hr>
<br>

### ✔ 스택과 큐의 java 코드 및 실행 결과
![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/0723f5df-0e0b-44f1-86ec-92d0da44f4cc)

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/22ab877e-7eca-4b33-b7ed-34941a35619d)
<br>
<hr>
<br>

**Reference**<br>

[널널한 개발자 : 넓고 얕게 외워서 컴공 전공자되기](https://www.inflearn.com/course/%EB%84%93%EA%B3%A0%EC%96%95%EA%B2%8C-%EC%BB%B4%EA%B3%B5-%EC%A0%84%EA%B3%B5%EC%9E%90/dashboard)<br>
[by devuna : 스택, 큐 개념/비교 /활용 예시](https://devuna.tistory.com/22)
