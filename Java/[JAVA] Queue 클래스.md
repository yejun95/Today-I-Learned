# 자바의 Queue 클래스
### Queue란?
- 데이터를 이릿적으로 쌓아두기 위한 자료구조로 스택과는 다르게 FIFO(First In First Out)의 형태를 가진다.

- 즉, 먼저 들어온 데이터가 가장 먼저 나가는 구조

<img width="982" height="453" alt="image" src="https://github.com/user-attachments/assets/c15165cf-02c3-4bc0-a791-6326de524469" /><br>
> Enqueue : 큐 맨 뒤에 데이터 추가<br>
> Dequeue : 큐 맨 앞쪽의 데이터 삭제
<br>
<hr>
<br>

## ✔️ Queue의 특징
- 먼저 들어간 자료가 먼저 나오는 FIFO 구조

- 큐의 한 쪽 끝은 프런트(front)로 정하여 삭제 연산만 수행

- 다른 한 쪽 끝은 리어(rear)로 정하여 삽입 연산만 수행

- 그래프의 넓이 우선 탐색(BFS)에서 사용

- 컴퓨터 버퍼에서 주로 사용, 마구 입력이 되었으나 처리를 하지 못할 때, 버퍼(큐)를 만들어 대기 시킴
<br>
<hr>
<br>

## ✔️ Queue 사용법
### Queue 선언
```java
Queue<Integer> intQueue = new LinkedList<>(); // int형 queue 선언
Queue<String> StrQueue = new LinkedList<>(); // String형 queue 선언
```
<br>
<br>

### 자바에서 큐를 `LinkedList`를 활용하여 생성하는 이유
- 큐는 인터페이스이기 때문에 자체로 객체를 만들 수 없고(new Queue<>() 불가능), 반드시 그 인터페이스를 구현한 클래스로 생성해야함

- LinkedList 클래스는 List 인터페이스뿐만 아니라 Deque, Queue 인터페이스도 구현 가능

- 그래서 Queue 타입으로 변수를 선언하고, new LinkedList<>()로 객체를 만들면, offer(), poll(), peek() 같은 큐 연산을 바로 쓸 수 있다.

- 그러나 LinkedList만 써야 하는 건 아님
  - PriorityQueue : 우선순위 기반의 큐 (정렬된 순서대로 poll 됨)
  - ArrayDeque : 배열 기반의 큐, LinkedList보다 빠른 경우가 많음
  - 즉, "Queue를 선언할 때 반드시 LinkedList여야 한다"는 건 아니고,
단순히 "FIFO 큐" 구조가 필요할 때 가장 쉽게 쓰는 구현체가 LinkedList라서 예제로 자주 등장

```java
Queue<Integer> intQueue = new LinkedList<>();
intQueue.offer(10);  // [10]
intQueue.offer(20);  // [10, 20]

System.out.println(intQueue.poll()); // 10 출력, 큐 = [20]
System.out.println(intQueue.peek()); // 20 출력, 큐 = [20] (변화 없음)
```
<br>

- `poll`과 `peek` 메서드는 동일하게 값을 출력해주지만 아래와 같은 차이가 존재

- `poll` :
  - 동작: 큐의 첫 번째 요소를 **반환하고 제거**
  - 큐 상태: 요소가 제거되어 큐 크기가 1 감소
  - 반환값: 제거된 요소 (큐가 비어있으면 null)
<br>

- `peek` : 
  - 동작: 큐의 첫 번째 요소를 **반환만 하고 제거하지 않음**
  - 큐 상태: 큐의 크기와 내용이 그대로 유지
  - 반환값: 첫 번째 요소 (큐가 비어있으면 null)
<br>
<br>

### Queue 값 추가
```java
Queue<Integer> stack = new LinkedList<>(); //int형 queue 선언
queue.add(1);     // queue에 값 1 추가
queue.add(2);     // queue에 값 2 추가
queue.offer(3);   // queue에 값 3 추가
```
<br>

- `add`와 `offer` 둘 다 동일하게 큐에 새로운 요소를 넣는 메서드지만, 가득 찼을 때 동작이 다르다.

| 메서드       | 성공 시      | 큐가 가득 찼을 때                     |
| --------- | --------- | ------------------------------ |
| `add()`   | `true` 반환 | 예외(`IllegalStateException`) 발생 |
| `offer()` | `true` 반환 | `false` 반환 (안전함)               |
<br>

**값 추가에 대한 현실적인 이견**
- LinkedList 같은 무한 큐(크기 제한 없음)를 쓰면 둘 차이가 없음 → 항상 성공.

- ArrayBlockingQueue 같은 크기 제한 있는 큐를 쓸 때는 차이가 발생.
  - add() → 꽉 차면 예외
  - offer() → 꽉 차면 false

- 안전하게 쓰기 위해서는 `offer`를 권장
<br>
<br>


### Queue 삭제
```java
Queue<Integer> queue = new LinkedList<>();
queue.offer(1);     // [1]
queue.offer(2);     // [1, 2]
queue.offer(3);     // [1, 2, 3]

// poll() 사용
Integer value1 = queue.poll();  // value1 = 1, queue = [2, 3]

// remove() 사용
Integer value2 = queue.remove(); // value2 = 2, queue = [3]

// clear() 사용
queue.clear();      // queue = []
```
<br>

- `poll()`
  - 동작 : 큐의 첫 번째 요소를 반환하고 제거
  - 반환값 : 제거된 요소 (큐가 비어있으면 `null` 반환)
  - 안전성 : 큐가 비어있어도 예외 발생하지 않음
<br>

- `remove()`
  - 동작 : 큐의 첫 번째 요소를 반환하고 제거
  - 반환값 : 제거된 요소
  - 예외 : 큐가 비어있다면 `NoSuchElementException` 발생
  - 주의 : 큐가 비어있지 않다는 것을 확신할 때 사용
<br>

- `clear()` : 큐의 모든 요소를 제거하고, 별도의 반환값이 없음
<br>
<br>

### 이해를 돕기 위한 그림
<img width="456" height="542" alt="image" src="https://github.com/user-attachments/assets/508309dd-207d-4a8a-9598-0840e877977c" />
<br>
<hr>
<br>

**Reference**<br>

[코딩팩토리 : 자바 Queue 클래스 사용법 & 예제 총정리](https://coding-factory.tistory.com/602)<br>
[Stranger's LAB : 배열을 이용한 Queue 구현하기](https://st-lab.tistory.com/183)
