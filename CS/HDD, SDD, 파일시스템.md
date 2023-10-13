### ✔ HDD, SSD
- Hard Disk Drive

- Solid-State Drive (solid-state, 진공을 대체한 고체 소자)

- 보조기억장치로써 track과 sector의 개념을 통해 데이터를 저장한다.
  - HDD, SSD 둘다 저장 방법은 동일하지만 SSD에서는 플래터  ->  칩으로 바뀐 것이다. 
<br>

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/86d2ce9f-cac0-4764-bde8-01fae8f63a2f)
<br>
<br>

- 같은 섹터에 여러번 Write를 할 수 있는데, 이를 overWrite라고 한다.
  - 기본적으로 섹터를 0으로 만들고 데이터를 새로 write 하는 것이 아니기 때문이다.
  - overWrite가 약 10만번 이상 진행되면 해당 섹터는 죽게 된다.  ->  베드섹터
<br>

- 섹터 1개의 기본 데이터 저장 값은 512byte이다.
  - 해당 값보다 적거나 많은 데이터가 들어 올 수 있게 된다.
  - ex) 적은 데이터 (빨간색)  ->  섹터 일부분만 차지, 그러나 섹터 1개를 다 쓴다.
  - ex) 초과 데이터 (파란색)  ->  다수의 섹터 사용, 이때 다음 섹터를 누군가 쓰고 있으면 그 다음 섹터에 저장
 <br>
 
![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/fb23735b-4372-47a5-8277-2dfc3b8a1dd1)
<br>

- 위의 사진을 보면 적은 데이터, 초과 데이터 모두 섹터를 낭비하고 있다.

- 특히 초과 데이터의 경우 섹터가 연속적이지 않기 때문에 HDD의 Actuator Arm이 한바퀴를 더 돌아야 한다.
  - 매우 비효율적
<br>

- 이를 해소하기 위한 것이 '디스크 조각 모음'이다.
  - 낭비되는 섹터를 합쳐준다.
  - SSD는 회전을 통해 암으로 데이터를 읽는 플래터가 아니고 칩이기 때문에 조각 모음이 필요없다.
<br>

**저장되는 형식**
- File Allocation Table (FAT) 라는 곳에 저장된다.

- 파일 삭제 시, 섹터가 0이 되는 것이 아니고 deleted라는 컬럼에 체크 표시가 되는 것이다.
  - 즉, 섹터에는 그대로 남아있다.
  - 이를 토대로 하드디스크의 복원 작업을 할 수도 있다.
<br>

- 0번 트랙, 0번 섹터 : MBR (Master Boot Record)의 자리로 OS의 부트 로더가 돌아간다.
  - 해당 트랙과 섹터의 데이터가 지워지면 당연하게도 부팅이 안된다.
<br>

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/d607c06c-d52e-43b5-9ae7-94160a21dd69)
<br>
<hr>
<br>

### ✔ 파일시스템 (File System)
- 파일이 저장되는 방법

- FAT32, NTFS, exFAT 이 3가지로 나뉘는데, 각각 장단점이 존재한다.
  - 외장 하드를 포맷할 때, 어떤 형식으로 포맷하겠는지 선택을 할 수 있다.
  - 이 때 저 셋 중 고르게 되며, 단순히 어떤 형식으로 저장할 것인가를 고르는 것이다.
  - 윈도우 사용자의 경우 NTFS가 가장 무난하다.
<br>

 - 포맷 : FAT를 클리어 하는것
  - 윈도우 부팅 usb를 NTFS로 설정하여 포맷한다는 것  ->  하드디스크의 FAT를 NTFS 형식으로 사용하겠다.
  - 빠른 포맷 : FAT의 메타 데이터만 날린다.  ->  섹터에 데이터가 남아있어 복원이 가능하다.
  - 느린 포맷 : 전체 트랙과 섹터를 찾아 전부 0으로 overWrite 시키는 것

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/2d816b3c-d584-4de4-ba83-a865022f46b8)
<br>
<hr>
<br>

**Reference**<br>

[널널한 개발자 : 넓고 얕게 배워서 컴공 전공자 되기](https://www.inflearn.com/course/%EB%84%93%EA%B3%A0%EC%96%95%EA%B2%8C-%EC%BB%B4%EA%B3%B5-%EC%A0%84%EA%B3%B5%EC%9E%90/dashboard)<br>
[iboxcomein : 파일시스템 NTFS, FAT32, exFAT 차이점과 특징 비교](https://iboxcomein.com/file-system-ntfs-fat32-exfat/)
