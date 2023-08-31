## Virtual Memory Management란 무엇인가?
- 메모리가 실제 메모리보다 많아 보이게 하는 기술로, 어떤 프로세스가 실행될 때 메모리에 해당 프로세스 전체가 올라가지 않더라도<br>
실행이 가능하다는 점에 착안하여 고안된 메모리 기법이다.
<br>
<hr>
<br>

### ✔ 등장 배경
- 기존 프로세스 실행은 코드 전체를 메모리에 로드하지만, 실제로 사용되는 부분은 일부분이여서 메모리 사용이 매우 비효율적이였고<br>
RAM의 용량이 부족하면 애플리케이션을 실행할 수 없었다.

- 이러한 물리적 한계를 극복하기 위해 나온 것이 가상메모리(Virtual Memory)이다.

- 프로세스 전체가 메모리에 올라오지 않아도 실행이 가능하기 때문에 물리 메모리보다 큰 프로세스나, 작은 여러개의 프로세스를 실행시켜<br>
사용자에게 무한대의 메모리가 있게끔 느끼도록 하는것이다.

- 이렇게 현재 필요한 page만 메모리에 올리는 것을 `Demand Paging`이라고 한다.
<br>
<hr>
<br>

### ✔ Demand Paging
-  실제로 필요할 때 page를 메모리에 올리는 것이다.

- page table에서 해당 page가 메모리에 있는지를 나타내는 `valid-invalid bit`를 사용한다.<br>
bit가 invalid인 경우 페이지가 물리적 메모리에 없다는 것이다.<br>
따라서 처음에는 모든 page entry가 invalid로 초기화되어있고, 주소 변환 시 bit가 invalid로 되어있다면 `page fault`라는 오류가 발생한다
<br>

**주소 변환과정**
1. 하드웨어가 TLB를 확인한다.

2. TLB hit인 경우 곧바로 주소를 변환하고, TLB miss인 경우 page table을 확인한다(→3).

3. page table의 valid-invalid bit가 valid로 되어 있다면 주소를 변환하고 TLB에 page를 올린다. invalid라면 page fault가 발생한다(→4).

4. page fault가 발생하면 MMU가 운영체제에 Trap을 걸고 커널 모드로 들어가서 page fault handler가 invoke된다.

5. 유효하지 않은 참조인 경우 프로세스를 종료시키고, 그렇지 않다면 빈 page frame을 얻는다. 빈 frame이 없다면 메모리에서 victim page를 선택하여 대체한다.  

6. 운영체제는 참조된 page를 디스크에서 메모리로 로드(I/O)하고, disk I/O가 끝날 때까지 이 프로세스는 CPU를 빼앗긴다.

7. disk I/O가 끝나면 page table이 업데이트되고 valid-invalid bit가 valid로 바뀐다. 그리고 ready queue에 프로세스를 넣어준다.   

8. 프로세스가 CPU를 잡게 되면 다시 이어서 수행한다. 
<br>

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/e48fe9b6-364e-4b0e-add8-111e8b5887ce)
<br>

💡 page 교체에 관한 알고리즘(replacement algorithm) : OPT, FIFO, LRU, LFU 등이 있다.
<br>
<hr>
<br>


**Reference**<br>

[인성의 개발노트 : 가상메모리란?](https://superohinsung.tistory.com/106)<br>
[Rebro의 코딩 일기장 : 가상 메모리](https://rebro.kr/179)<br>
[yein : 가상 메모리](https://ahnanne.tistory.com/15)<br>



