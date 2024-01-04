## VM arguments를 이용하여 실행 서버 선택하기
- VM arguments : JVM의 동작 방식을 변경하는 값이다.
  - Ex) JVM 힙 메모리 용량 설정, 전역 데이터 설정 등
<br>

- 이러한 VM arguments를 이용하면 전역 데이터 설정 및 properties에 관련된 환경 설정도 가능하다.
  - Vue.js의 main.js에서 global 변수를 설정하는 것과 비슷하다고 생각하였다.
<br>
<hr>
<br>

### ✔ 설정 예시
- 이클립스 기준 상단 카테고리에서 접근이 가능하다.
  - Run -> Run Configrations
<br>

- 해당 창을 열면 아래와 같은 화면이 나오고 Spring Boot 탭에서 적용할 프로젝트 설정이 가능하다.
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/a6f257ba-bd76-406b-aea5-161a942cdee5)
<br>

- Arguments 탭에서 JVM 설정이 가능하다.
  - 앞서 말했던 전역 데이터 및 환경 설정을 해당 탭에서 하는 것이다. 
  - `test`라는 전역 데이터를 설정하여 main 메서드에서 실행하여보자.
  - `-D` 옵션 : 시스템 등록 정보 변수를 이름/값 쌍으로 설정

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/11207de9-894f-4895-8290-efb6c46e7d5d)
<br>
<br>

- main 메서드에서 해당 시스템 변수 실행
  - `test`라는 key로 내가 설정한 데이터가 출력되는 것을 알 수 있다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/95891af7-17b3-408a-8dd4-68ec5ea086ec)
<br>
<br>

- `spring.profiles.active` 설정을 부여하여 `application.properties`에서 한 것 처럼 실행 서버 설정도 가능하다.
  - `application-prod.properties` 에 설정된 포트를 실행해보겠다.
  - 아래 사진을 보면 prod port 실행이 정상적으로 되고 있다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/f9663c15-b5b1-49b2-94b7-262c33ac0deb)

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/688cb733-850a-4ae7-b37c-276dffed7d7e)
<br>
<hr>
<br>

### ✔ IDE가 없는 환경에서 spring.profiles.active 설정하기
- 지금까지 이클립스를 통한 GUI 환경에서 VM arguments를 설정하였다.

- 하지만 GUI가 없는 환경에서 IDE를 실행하지 못하였을 때, 개발, 운영 서버 선택을 어떻게 할 것인가?
  - 이는 톰캣 설정을 변경하여 가능한 부분이다. (내장 톰캣일 경우 설정이 불가능) 
  - 기본적으로 springboot는 내장 톰캣이기 때문에 해당 설정을 하기 위해서는 외부에서 tomcat을 따로 설정해야 한다.
<br>
<hr>
<br>

- [SpringBoot에서 외장 톰캣 설정 방법](https://github.com/yejun95/Today-I-Learned/blob/master/SpringBoot/SpringBoot%EC%97%90%EC%84%9C%20%EC%99%B8%EC%9E%A5%20%ED%86%B0%EC%BA%A3%20%EC%84%A4%EC%A0%95%20%EB%B0%A9%EB%B2%95.md)<br>
- [외장 톰캣에서 profile 설정하기](https://github.com/yejun95/Today-I-Learned/blob/master/SpringBoot/%EC%99%B8%EB%B6%80%20%ED%86%B0%EC%BA%A3%EC%97%90%EC%84%9C%20profile%20%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0.md)

