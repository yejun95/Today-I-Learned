## 다이렉트X(DirectX)란 무엇인가?
- 마이크로 소프트사가 윈도즈용으로 개발한 멀티미디어 응용 프로그램 인터페이스이다.

- GPU를 제어하고 프로그래밍 하는데 쓰이는 저수준 그래픽API

- 즉, 각종 미디어를 사용한 응용 프로그램이 하드웨어 장치에 직접 접속 (direct access)하여 고속으로 처리 할 수 있도록 해주는 API라는 의미에서 다이렉트 X라는 이름이 붙여졌습니다.
<br>
<hr>
<br>

### ✔ 탄생 배경
- 기존에는 각종 미디어를 사용하기 위해 응용 프로그램들이 하드웨어 장치에 접속하기 위해서는 사용자 환경에서부터 커널 영역을 거쳐 하드웨어에 접속해야 한다.

- 그리고 하드웨어 장치가 주는 응답을 응용 프로그램에 출력하기 위해서는 다시 커널 영역을 지나 사용자 환경으로 거쳐가야하는 불편함이 있었다.

- 이와 같은 과정을 줄이기 위해 사용자 환경과 하드웨어 장치를 직접 연결하여 빠른 응답을 받을 수 있게 한 것이다.

- 이 때문에 DirectX라는 이름이 붙었다.
<br>
<hr>
<br>

### ✔ CPU vs GPU
![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/95617980-2b63-44d8-8a72-3a4a86c5f0e5)
<br>
<br>

**CPU**
- 매우 고급 인력이며 복잡한 연산을 처리

- ALU(산술연산장치)가 적어 많은 일은 하지 못한다
<br>
<Br>

**GPU**
- 병렬 구조로 간단한 연산을 많이 처리

- ALU(산술연산장치)가 굉장히 많다
  - 때문에 간단한 연산들은 GPU로 일을 넘겨 주어 처리해야 한다
  - 이를 DirectX에서 처리
<br>
<br>

[CPU와 GPU의 차이 예시 영상](https://www.youtube.com/watch?v=-P28LKWTzrI)
<br>
<hr>
<br>

**Reference**<br>

[잡지식닷컴 : Directx (다이렉트 X) 란 무엇일까?? 간단 설명](https://jobgisick8682-finale.tistory.com/37)<br>
[JH_Dev : [DirectX12] Chapter 1. DirectX란?](https://velog.io/@pilgyu14/DirectX12-Chapter-1.-DirectX%EB%9E%80)<br>
