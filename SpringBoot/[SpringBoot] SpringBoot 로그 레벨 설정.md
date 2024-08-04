## 로그 레벨(Log level)이란?
- 로그 메시지의 중요도를 나타내는 수준을 의미한다.

- 로그 레벨은 로깅 시스템에서 사용되며, 로그 메시지의 중요도에 따라 해당 메시지를 기록할지 결정하는 데 사용된다.
<br>
<hr>
<br>

### ✔ 로그 레벨 (log4js)
**TRACE**
- 가장 상세한 로그 레벨로, 애플리케이션의 실행 흐름과 디버깅 정보를 상세히 기록한다. 주로 디버깅 시에 사용된다.
<br>

**DEBUG**
- 디버깅 목적으로 사용되며, 개발 단계에서 상세한 정보를 기록한다.

- 애플리케이션의 내부 동작을 이해하고 문제를 분석하는 데 도움을 준다.
<br>

**INFO**
- 정보성 메시지를 기록한다.

- 애플리케이션의 주요 이벤트나 실행 상태에 대한 정보를 전달한다. 
<br>

**WARN**
- 경고성 메시지를 기록한다.

- 예상치 못한 문제나 잠재적인 오류 상황을 알리는 메시지이다.

- 애플리케이션이 정상적으로 동작하지만 주의가 필요한 상황을 알려준다.
<br>

**ERROR**
- 오류 메시지를 기록한다.

- 심각한 문제 또는 예외 상황을 나타내며, 애플리케이션의 정상적인 동작에 영향을 미칠 수 있는 문제를 알린다.
<br>

**FATAL**
- 가장 심각한 오류 메시지를 기록한다.

- 애플리케이션의 동작을 중단시킬 수 있는 치명적인 오류를 나타낸다.

- 일반적으로 이러한 오류는 복구가 불가능하거나 매우 어려운 상황을 의미한다.
<br>
<br>

**중요도 순서**
- TRACE > DEBUG > INFO > WARN > ERROR > FATAL

- 상위 로그 레벨은 하위 로그를 전부 포함한다.
  - Ex) INFO로 셋팅하면, INFO, WARN, ERROR, FATAL이 기록된다.
<br>
<br>

- 빨간선 기준으로 위는 개발자가 의도하지 않은 에러, 아래쪽은 개발자가 의도한 예외라고 보면 된다.
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/7a656837-4a30-47fb-8b46-1afcd26537cb)
<br>
<hr>
<br>

## 로그 레벨 적용
- 일반적으로 log4j 라이브러리를 활용한다.

- `application.properties`에서 설정이 가능하다.

```
// application.properties

logging.level.org.springframework={level 설정}
```

- INFO 설정 : 평소 로그 레벨 설정을 안해도 이부분 까지는 출력이 된다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/a53e707d-5055-4410-8cdb-90f7ee350679)
<br>
<br>

- DEBUG 설정 : DEBUG 로그 출력, 어노테이션의 매칭, 비매칭 로그까지 출력된다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/1a48be4b-2a53-4867-a030-4d6657323037)

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/5d047123-97ed-4b0a-a420-04a35a88b592)

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/9dbac0ce-a88a-4bc4-b3bd-71fe54a0a802)
<br>
<br>

- TRACE 설정 : DEBUG와 마찬가지로 어노테이션의 매칭, 비매칭 로그까지 출력된다.
  - DEBUG와의 차이를 정확히 모르겠다.
<br>
<hr>
<br>

### ✔ TRACE, DEBUG 차이
- log4js에서는 원래 TRACE 제외한 5가지가 전부였다.

- 1.2.12버전부터 TRACE가 추가된 것인데, DEBUG 레벨이 너무 광범위 하게만 나타나 좀 더 상세 정보를 주기 위해
추가되었다.
<br>
<hr>
<br>

### ✔ System.out.println() ??
- 로깅과 디버깅에 대해서도 무지한 나에게 에러가 발생했을 때나,성공을 하였을 때 `System.out.println`을
남겨두어 정상 작동인지, 에러인지 확인하는 방법만 알고 있었다.

- 때문에 로깅과 비교하여 뭐가 다른지 알아보자.

**로깅**
- 출력 형식 지정 가능

- 로그 레벨에 따라 남기고 싶은 로그를 별도로 지정 가능
  - `System.out.println`은 무조건 출력이 된다.
<br>

- 콘솔 뿐만 아니라 파일, 네트워크 등 로그를 별도에 위치에 남길 수 있다.
