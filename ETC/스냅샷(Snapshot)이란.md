## 스냅샷(Snapshot)이란?
- 사진을 찍듯이 특정 시간(시점)에 데이터 저장 장치(스토리지)의 파일 시스템을 포착해 별도의 파일이나 이미지로
저장, 보관하는 기술

- 해당 기능을 이용해 데이터를 저장하면 일정 시점의 상태로 데이터 복원 가능
  - Ex) Windows OS의 복원 지점과 같이 장애나 데이터 손상 시 스냅샷을 생성한 시점으로 데이터 복구 가능
<br>
<hr>
<br>

### ✔ 백업 vs 스냅샷
**백업**
- 원본 데이터를 그대로 복사해 다른 곳에 저장

- 스토리지 상에 동일한 공간이 필요
<br>

**스냅샷(Snapshot)**
- 초기 생성 혹은 데이터 변경이 있기 전까지 스토리지 공간 차지 X

- 메타데이터의 복사본에 해당하여 생성시 시간이 오래 걸리지 않음

- 장애가 발행해도 데이터 복원 가능
<br>
<hr>
<br>

### ✔ SpringBoot생성시 Snapshot
- SpringBoot에서 Snapshot이란 아직 개발이 완료되지 않은 버전을 의미

- 추가로 애플리케이션 개발 시 snapshot이라는 버전을 명시하면, 해당 애플리케이션의 현재 상태가 완료 버전이 아닌,
중간 단계의 버전이라는 의미로 사용할 수 있다.
<br>
<hr>
<br>

**Reference**<br>

[Dotori Planet : 스냅샷(Snapshot) 이란?](https://leinoi.tistory.com/8)<br>
[Inflearn : Snapshot 은 어떤 의미인가요?](https://www.inflearn.com/questions/268685/snapshot-%EC%9D%80-%EC%96%B4%EB%96%A4-%EC%9D%98%EB%AF%B8%EC%9D%B8%EA%B0%80%EC%9A%94)
