## .pem 파일과 .ppk 파일을 알아보자
- pem은 Privacy Enhanced Mail의 약어로 Base64의 인코딩으로 이루어진 인증서 파일이다.

- cloud를 통해 인스턴스를 생성하면 key pair를 .pem 파일 형식으로 발급을 받게 되는데,
이를 통해 SSH로 서버에 접속을 할 수가 있다.

- 그러나 SSH가 아닌 PuTTY를 사용하게 되면 .pem키로는 연결할 수가 없고, 이를
.ppk 파일로 변환해 연결해야 한다.
  - ppk의 뜻 자체가 PuTTY Private Key file 이기 때문에 PuTTY에서 사용한다는 것을 알 수 있다.
  - 변환을 하면 아래와 같이 나오게 된다.
<br>

![image](https://github.com/yejun95/Today-I-Learn/assets/121341413/77cecb78-90ff-4c5c-938c-f8d7608e24c8)

- 이처럼 변환하기 위해서는 PuTTYgen 프로그램을 사용하여 진행한다.
<br>

**💡 ppk로 PuTTY접속 시 나타나는 에러**
![image](https://github.com/bjsystems/contract/assets/121341413/a5ac4ac5-101e-401a-8141-742bb17d172f)
<br>

- `Unable to load key file "xxx.ppk" (PuTTY key format too  new)` 오류가 발생하는 원인은
putty version이 낮아서 발생하는 것으로
putty를 0.75 버전 이상을 사용하거나
아래와 같이 PuTTYgen에 대해 PPK 버전 2 으로 저장해서 사용하면 해결됨

- 초기에 PuTTYgen을 실행하면 버전 3으로 저장되어 있다.
<br>

![image](https://github.com/bjsystems/contract/assets/121341413/bdf3e199-dea9-42ae-879b-26d4746830d0)
<br>
<hr>
<br>

**Reference**<br>

[hyunsoft의 지식공간 : PuTTY key format too new 오류 해결 및 ppk 만드는 방법](https://hyunsoft.tistory.com/entry/ppk-%EB%A7%8C%EB%93%9C%EB%8A%94-%EB%B2%95-%EB%B0%8F-PuTTY-key-format-too-new-%EC%98%A4%EB%A5%98-%ED%95%B4%EA%B2%B0%EB%B2%95)
