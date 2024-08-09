### ✔ 클래스와 객체
- 클래스란 '객체를 정의해놓은 것' 또는 '객체의 설계도'라고 한다.
  - 연관되어진 변수와 메서드의 집합

- 객체는 클래스(설계도)를 통해 만드는 사물이라고 볼 수 있다.
  - 클래스에 선언된 모양 그대로 생성된 실체
  - 클래스의 '인스턴스(instance)'라고도 불린다.
  - 객체는 모든 인스턴스를 대표한다.
<br>

- 즉, 클래스라는 설계 도면을 먼저 그리고 이후 객체를 설계 도면안에 넣어주는 것이다.
  - ex) 자동차를 만들 수 있는 설계도(클래스)에 부품을 조립하여 객체를 생성한다.
  - ex) 붕어빵 틀이라는 설계도에 팥, 밀가루 등의 재료를 통해 객체를 생성  ->  붕어빵 탄생
<br>

```javascript
public class 붕어빵틀 {
	String 팥;
	String 슈크림;
	String 밀가루;
	String 물;
}

```
<br>

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/3c43ce4c-509e-48f2-9929-5e5213680652)
<br>
<hr>
<br>

### ✔ 객체의 구성요소
- 객체는 속성과 기능 두 종류의 구성 요소로 되어있다.
  - 속성 : TV의 크기, 길이, 높인, 채널 등
  - 기능 : 전원키기, 볼륨 올리기, 볼륨 내리기
<br>

- 다른 말로 속성은 '멤버변수', 기능은 '메서드'라고 한다.

```javascript
class TV{
  String color;
  boolean power;               <-----------------------------  변수
  int channel;

  void power() { power = !power; }
  void channelUp() { channel++;}          <------------------ 메서드
  void channelDown() { channel--;}
}
```
<br>
<hr>
<br>

### ✔ 인스턴스
- 클래스로부터 객체를 만드는 과정을 클래스의 인스턴스화라고 한다.
  - 위에서 붕어빵 틀을 통해 붕어빵을 만드는 과정이 인스턴스화이고, 그로 인해 객체가 생성되는 것이다.

- 어떤 클래스로부터 만들어진 객체를 해당 클래스의 인스턴스라고 한다.
<br>
<hr>
<br>

### ✔ 인스턴스의 생성과 사용
- 클래스명 변수명
  - 변수 선언 후 객체의 주소 저장
  - 선언과 동시에 주소 저장

```javascript
Tv t;
t = new TV();    <---------- 변수 선언 후 객체의 주소 저장

Tv t = new Tv();  <------------ 선언과 동시에 주소 저장 
```
<br>

- `Tv t = new TV()`를 통해 객체의 주소를 저장하는 것을 '인스턴스화'라고 한다.
  - TV는 객체다.
  - TV는 TV 클래스의 인스턴스다.
<br>

- 💡 new 연산자를 통해 객체의 주소를 참조변수에 저장하면 아래와 같이 메모리에 할당된다.
  - `0x100`이라는 주소에 저장 (실제 주소가 아닌 가정한 것)
  - 데이터(변수)들은 JVM 메모리영역에 저장
  - 참조형 변수는 힙 영역, 기본형 변수는 스택 영역에 저장된다.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/f959ebeb-cc91-4897-b043-0c140f4d0706)
<br>
<br>

**💡 인스턴스는 참조변수를 통해서만 다룰 수 있다.  ->  Why??** 
- 안정성 측면
  - 객체에 직접 접근하지 않고 변수를 통해 다루기 때문에 메모리에 잘못된 접근을 막을 수 있다.
  - ex) 자바는 메모리를 가비지 컬렉터를 통해 관리하는데, 객체를 직접 다루면 메모리를 사용자가 직접 관리해야 되므로<br>
       메모리 누수 등의 비효율적인 측면이 발생할 수 있다.

  - 가비지 컬렉터 : 객체를 생성하면 자동으로 메모리에 할당되며, 해당 객체를 더 이상 사용하지 않을 때는<br>
     개발자가 직접 할당 해제를 하지 않아도 객체를 제거해준다.
<br>
<hr>
<br>

### ✔ 객체 배열
- `TV tv1, tv2, tv3` -> `Tv[] tvArr = new TV[3]`

- 💡 객체 배열은 생성 후에 각 요소에 저장을 필히 해줘야한다.

```javascript
Tv[] tvArr = new Tv[3];

tvArr[0] = new Tv();
tvArr[1] = new Tv();
tvArr[2] = new Tv();
```
<br>
<hr>
<br>

### ✔ 클래스의 또 다른 정의
- 클래스는 데이터와 함수의 결합이다. (구조체 + 함수)
  - 구조체 : 데이터 종류의 관계없이 하나의 집합으로 저장할 수 있는 공간
 
- 즉, 클래스는 타입이 다른 데이터와 함수를 모두 가질 수 있기 때문에 사용자 스스로 정의할 수 있다.
  
```javascript
int hour;    // 시간을 표현하기 위해 정의한 변수
int minute;    // 분을 표현하기 위해 정의한 변수
float second;    // 초을 표현하기 위해 정의한 변수
```
<br>

- 해당 변수들은 제어자를 통해 기본값을 설정할 수 있다.
  - getter, setter
 
```javascript
public class Time {
	private int hour;
	private int minute;
	private float second;
	
	public int getHour() { return hour; }
	public int getMinute() { return minute;	}
	public float getSecond() { return second; }
	
	public void setHour(int h) {
		if (h < 0 || h > 23) return;
		hour = h;
	}
	
	public void setMinute(int m) {
		if (m < 0 || m > 59) return;
		minute = m;
	}
	public void setSecond(float s) {
		if (s < 0.0f || s > 59.99f) return;
		second = s;
	}
}
```
 <br>
 <hr>
 <br>

**Reference**<br>

[자바의 정석 3판 : chapter 06 객체지향 프로그래밍]<br>
[murpin_kim.log : 가비지 컬렉터에서의 메모리 누수는 언제 일어날까?](https://velog.io/@muman_kim/JavaScript-%EA%B0%80%EB%B9%84%EC%A7%80-%EC%BB%AC%EB%A0%89%ED%84%B0%EC%97%90%EC%84%9C%EC%9D%98-%EB%A9%94%EB%AA%A8%EB%A6%AC-%EB%88%84%EC%88%98%EB%8A%94-%EC%96%B8%EC%A0%9C-%EC%9D%BC%EC%96%B4%EB%82%A0%EA%B9%8CGarbage-Collector)
  
