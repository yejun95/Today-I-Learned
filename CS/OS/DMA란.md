### ✔ 입출력 제어 방식
- 프로그램에 의한 I/O (CPU 개입 O)

- 인터럽트에 의한 I/O (CPU 개입 O)

- DMA에 의한 I/O (CPU 개입 X)

- 채널에 의한 I/O (CPU 개입 X)
<br>
<hr>
<br>

## DMA(Dirct Memory Access)란 무엇인가?
- 주변장치(하드디스크, 그래픽카드 등)들이 메모리에 직접 접근하여 읽거나 쓸 수 있도록 하는 기능이다.

- 즉, CPU의 개입 없이 I/O 장치와 기억장치 사이의 데이터를 전송하는 접근 방식이다.
<br>
<hr>
<br>

### ✔ 등장 배경
- 기존 PIO(Programmed Input/Output)은 CPU가 주변장치와 데이터를 주고받는 방식으로 효율이 떨어진다.
  - 주변장치와 데이터를 주고 받기 위해 사용자환경 > 커널 > 하드웨어에 이르는 절차를 전부 밟기 때문

- 위와 같은 불필요 절차를 없애기 위해 DMA가 IBM에서 개발되었다.
<br>

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/8759f5b1-d480-4369-94b4-7cf66c1f9943)
<br>
<hr>
<br>

### ✔ 주요 내용
- CPU 개입이 없다는 것이 중요한 포인트이다.
  - CPU가 해야할 주변장치와의 데이터 전송을 DMA controller가 해주는 것
<br>

- 고속의 I/O 장치의 경우 빈번한 인터럽트가 발생하는데, DMA를 사용함으로써 Data I/O 처리가 끝나지 않았음에도
CPU는 대기하지 않아도 된다.

- 또한 인터럽트가 자주 걸리는 것을 막기 위해 I/O 로컬 버퍼에 들어가 있는 작은 크기의 데이터가<br>
특정 블록 단위까지 찼을때까지 기다렸다가 DMA가 직접 메모리에 카피해주는 작업까지 해주고<br>
해당 작업이 끝났을 때, CPU한테 인터럽트를 거는 방식을 취한다.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/753f6838-d142-44fb-a898-ede650c67fd3)
<br>
<br>

### ✔ DMA가 접근하는 방식
- DMA를 제어하는 DMAC(Direct Memory Access Control)가 있다.

- CPU가 DMAC한테 주소, 데이터 길디 등을 알려주면 DMAC가 처리 후 HW 인터럽트로 처리가 끝났음을 알린다.

- 즉, DMAC가 데이터를 처리하는 동안 CPU는 다른 일을 할 수 있다.
<br>
<hr>
<br>

**Reference**<br>

[편하게 보는 전자공학 블로그 : DMA(Direct Memomry Access)란? (+PIO, 채널제어방식](https://kkhipp.tistory.com/168)<br>
[천천히올라가자 개발 블로그 : 개발자도 알면 좋은 DMA](https://ksk-developer.tistory.com/40)<br>
[I'm always a beginner : DMA란?](https://park-duck.tistory.com/entry/DMA%EB%9E%80-Direct-Memory-Access)
