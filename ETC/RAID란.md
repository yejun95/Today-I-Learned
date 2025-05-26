# RAID란 무엇인가?
- Redundant Array of Independent Disk (독립된 디스크의 복수 배열)<br>
혹은 Redundant Array of Inexpensive Disk (저렴한 디스크의 복수 배열)

- 말 그대로 RAID는 여러 개의 디스크를 묶어 하나의 디스크처럼 사용하는 기술

- 컴퓨터를 구성하는 여러 부품 중 기계적인 특성 때문에 상대적으로 속도가 느린<br>
하드디스크를 보완하기 위해 만든 기술

![image](https://github.com/user-attachments/assets/66e74c80-38f0-4390-a3f3-a8a8b663cf2f)
<br>
<hr>
<br>

## ✔️ 기대 효과
- 대용량의 단일 볼륨을 사용하는 효과

- 디스크 I/O 병렬화로 인한 성능 향상 (RAID 0, RAID 5, RAID 6 등)

- 데이터 복제로 인한 안정성 향상 (RAID 1 등)
<br>
<hr>
<br>

## ✔️ 구성 방식
- RAID 0 ~ RAID 6까지 있지만, 최근 출시되는 RAID 컨트롤러에서 사용 가능 한 RAID Level은 RAID 0, RAID 1, RAID 5, RAID 6 이다.
<br>

### RAID 0
![image](https://github.com/user-attachments/assets/7ece1559-8a98-4051-b606-f10ab08c6d7c)
<br>

- striping (스트라이핑) 방식

- 최소 2개의 디스크가 필요하며 RAID를 구성하는 모든 디스크에 데이터를 분할하여 저장

- 전체 디스크를 모두 동시에 사용하기 때문에 성능은 단일 디스크의 N배이며 용량 역시 N배

- 하지만 하나의 디스크라도 문제가 발생할 경우 전체 RAID가 깨지는 일이 발생하기 때문에 안정성은 1/N으로 줄어듬
<br>
<br>

### RAID 1
![image](https://github.com/user-attachments/assets/386f2b6a-39db-41b6-bd87-6a486d46a86f)
<br>

- Mirroring (미러링) 방식

- 최소 2개의 디스크가 필요하며 RAID 1은 모든 디스크에 데이터를 복제하여 기록

- 즉, 동일한 데이터를 N개로 복제하여 각 디스크에 저장하는 방식
  - 여러 개의 디스크로 RAID를 구성해도 실제 사용 가능한 용량은 단일 디스크의 용량과 동일
<br>

- 최대 강점은 안정성이 높다는 것이지만 비용 문제로 인해 거의 사용하지 않는다.
<br>
<br>

### RAID 2
![image](https://github.com/user-attachments/assets/bf3daf0b-2452-4062-ad0d-5c232c8ec23e)
<br>

- 현재 사용하지 않는 RAID Level

- bit 단위로 striping하고, error correction을 위해 Hamming code를 사용
<br>
<br>

### RAID 3
![image](https://github.com/user-attachments/assets/45bab3e3-756e-4e5d-b62f-d63c0689d9a5)
<br>

- 현재 사용하지 않는 RAID Level

- Byte 단위로 striping하고, error correction을 위해 패리티 디스크를 1개 사용
<br>
<br>

### RAID 4
![image](https://github.com/user-attachments/assets/370d0974-c254-41fb-a9f5-d10c29f9588b)
<br>

- 현재는 (거의) 사용하지 않는 RAID Level

- block 단위로 striping을 하고 error correction을 위해 패리티 디스크를 1개 사용

- 용량 및 성능이 단일 디스크 대비 N-1배 증가하며, 최소 3개의 디스크로 구성이 가능

- 1개의 디스크만 에러 시 복구 가능 (2개 이상의 디스크 에러 시 복구 불가능)
<br>
<br>

### RAID 5
![image](https://github.com/user-attachments/assets/2b3568ae-d9d4-4f6e-b248-36d5ad926f6b)
<br>

- 제일 사용 빈도가 높은 RAID Level

- block 단위로 striping을 하고 error correction을 위해 패리티를 1개의 디스크에 저장

- 단, 패리티 저장은 고정된 디스크에 하지 않고, 매번 다른 디스크에 저장 (RAID 4 단점 개선)

- 용량 및 성능이 단일 디스크 대비 N-1배 증가하며, 최소 3개의 디스크로 구성이 가능
<br>
<br>

### RAID 6
![image](https://github.com/user-attachments/assets/529ba392-cab0-4fd7-94f8-d6ba8baf9835)
<br>

- RAID 5에서 성능과 용량을 좀 더 줄이고 안정성을 높인 방식

- block 단위로 striping을 하고 error correction을 위해 패리티를 2개의 디스크에 저장
  - 매번 다른 디스크에 패리티를 저장
<br>

- 용량 및 성능이 단일 디스크 대비 N-2배 증가하며, 최소 4개의 디스크로 구성이 가능

- RAID 5에서 성능과 용량을 줄이는 대신 안정성을 높인 방식
<br>
<hr>
<br>

**Reference**<br>

[Alpaca : RAID 란?, RAID 구성방식(RAID 0, 1, 4, 5, 6, 1+0, 0+1)](https://velog.io/@zxcvbnm5288/RAID-%EB%9E%80-RAID-%EA%B5%AC%EC%84%B1%EB%B0%A9%EC%8B%9DRAID-0-1-4-5-6-10-01)<br>
[DEVOCEAN : RAID 정리 1. RAID 기본 설명 및 RAID Level (레이드 레벨)](https://devocean.sk.com/blog/techBoardDetail.do?ID=163608)
