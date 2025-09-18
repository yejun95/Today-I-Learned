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
Queue<Integer> intQueue = new LinkedList<>(); // int형 queue 선언
intQueue.offer(10);
intQueue.offer(20);

System.out.println(intQueue.poll()); // 10
System.out.println(intQueue.peek()); // 20
```



<br>
<hr>
<br>

**Reference**<br>

[코딩팩토리 : 자바 Queue 클래스 사용법 & 예제 총정리]
