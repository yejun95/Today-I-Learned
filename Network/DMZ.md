# DMZ란?

- 내부 네트워크에 존재하지만 외부에서 접근할 수 있는 특수한 네트워크 영역

- 외부 네트워크와 내부 네트워크 사이에서 외부 네트워크 서비스를 제공하고 내부 네트워크를 보호하는 서브넷,<br>
즉 외부에 오픈된 서버 영역을 의미한다.
<br>

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/6934b50f-49bc-46ac-a787-f0a65365fbef)
<br>
<br>

- DMZ의 앞뒤로 방화벽이 설치된다. 하나는 내부 네트워크와 다른 하나는 외부 네트워크와 연결된다.<br>

- 내/외부 네트워크는 DMZ에 접속할 수 있지만,  DMZ내의 컴퓨터는 오직 외부 네트워크에만 연결할 수 있다.<br>
이는DMZ 내의 호스트의 침입으로부터 내부 네트워크를 보호하기 위함이다.
<br>
<hr>

### **✔ 사용 이유**

- 컴퓨터와 네트워크 사용 기관은 보안 문제로 인하여 LAN(local Area Network)만 사용<br>
이 경우 외부 네트워크와 단절되어 이메일, DNS, FTP 등 인터넷 서비스 이용 불가

- DMZ를 중간에 위치시켜 내부 네트워크를 보호하면서도 외부 네트워크를 이용하기 위함
<br>
<hr>

### ✔ 활용 예제

- A : 외부에서 접속해야하는 시스템

- B : 회사 내부망에서 사용하는 시스템

A => B : 내부망으로의 접속은 차단<br>
B => A : 외부 네트워크의 기능을 사용하기 위하여 접근 허가    
<br>

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/f5b85b36-ca66-4a6b-9b12-e12c14fe0e6d)
<br>
<br>

- 위에서 말한 활용 예제는 이메일서버, FTP 서버를 위해 주로 사용하기도 한다.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/cb2542ba-3c71-46a8-80b9-461f910607a4)
<br>
<br>

- 아래 이미지는 추가적인 이미지

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/f0d7fe0c-54dc-40f4-8d74-133e6044f921)
<br>
<br>






**참고 자료**<br>
[끄적끄적 : DMZ](https://ee-22-joo.tistory.com/40)<br>
[오늘의 기록 : DMZ 의미와 뜻](https://hyeri0903.tistory.com/225)
[(주)이노비스 공식 블로그 : DMZ 이란 무엇이고 어떻게 활용하는가?](https://m.blog.naver.com/innoviss/222246852119)
