## Casting
- 하나의 데이터 타입을 다른 타입으로 바꾸는 것을 타입 변환 혹은 형변환(캐스팅) 이라고 한다.

- 자바의 데이터형을 알아보면 크게 두가지로 나뉘게 된다.
  - 기본형(primitive type) - Boolean Type(boolean) - Numeric Type(short, int, long, float, double, char)
  - 참조형(reference type) - Class Type - Interface Type - Array Type - Enum Type - 그 외 다른 것들
<br>

- 기본형(primitive) 이든 참조형(referece) 이든 하나의 타입이다. 이는 즉, 서로 타입간의 형변환(casting)이 가능하다는 말이다.

- 기본적으로 자바에선 대입 연산자 = 에서 변수 와 값 서로 양쪽의 타입이 일치하지 않으면 할당이 불가능하다.
 - 프로그램에서 값의 대입이나 연산을 수행할 때는 같은 타입끼리만 가능하기 때문이다.

```javascript
long d = 10.233; // ERROR

long d = (long)10.233;
```
<br>
<br>

- 기본 타입이 형변환이 가능하듯, 자바의 **상속 관계**에 있는 부모와 자식 클래스 간에는 서로 간의 형변환이 가능하다.

- 자식 클래스의 객체는 부모 클래스를 상속하고 있기 때문에 부모의 멤버를 모두 가지고 있다.
  - 반면 부모 클래스의 객체는 자식 클래스의 멤버를 모두 가지고 있지는 않는다.
  - 💡 즉, 참조변수의 형변환은 사용할 수 있는 멤버의 갯수를 조절하는 것이다.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/24a283e4-fb53-4dc9-966c-c0ba2e0b9a87)

```javascript
class Parent {
    String name;
    int age;
}

class Child extends Parent {
	/*
    String name;
    int age;
    */
    int number;
}

Parent p = new Parent(); 
Child c = new Child();

Parent p2 = (Parent)c; // 업캐스팅 - 자식에서 부모로
Child c2 = (Child)p2; // 다운캐스팅 - 부모에서 자식으로
```
<br>
<br>

- 주의점 : 같은 부모 클래스를 상속받고 있더라도 형제 클래스 끼리는 아예 타입이 다르기 때문에 참조 형변환 불가능하다.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/b59e862c-c932-4cad-bfbb-8e6b33277625)
<br>
<hr>
<br>

### ✔ UpCasting
- 자식 클래스가 부모 클래스 타입으로 캐스팅 되는 것이다.

- 업캐스팅은 캐스팅 연산자 괄호를 생략할 수 있다.
  - 가능한 이유 : 멤버의 갯수가 많은 것 -> 적은 것으로 변환되기 때문.<br>
  자식 객체 : 부모의 멤버까지 포함되어 더 많은 것이 당연하다.
  - 원시타입 형변환때도 보면 큰 데이터 타입 -> 작은 데이터 타입으로 형변환 시 생략이 됐었다.

```javascript
public class Main {
    public static void main(String[] args) {
        int intValue = 10; // int 타입 변수 선언 및 초기화
        double doubleValue = intValue; // int를 double로 형변환, 괄호 생략 가능
    }
}
```
<br>

- 부모 클래스로 캐스팅 된다는 것은 멤버의 갯수 감소를 의미한다.
  - 이는 곧 자식 클래스에서만 있는 속성과 메서드는 실행하지 못한다는 뜻이다.
<br>

- 업캐스팅을 하고 메소드를 실행할때, 만일 자식 클래스에서 오버라이딩한 메서드가 있을 경우,<br>
부모 클래스의 메서드가 아닌 오버라이딩 된 메서드가 실행되게 된다.
<br>
<br>

**업캐스팅 멤버 제한**
- 업캐스팅에는 주의해야 할 점이 있다. 바로 멤버 갯수 감소로 인한 멤버 접근 제한이다.

- 부모를 상속해서 멤버가 많은 자식 클래스에서 부모 클래스로 업캐스팅 했으니 당연히 멤버 갯수가 감소하게 된다.<br>
그리고 이는 실행할 수 있는 속성과 메서드가 제한된다는 뜻이기도 하다.

```javascript
class Unit {
    public void attack() {
        System.out.println("유닛 공격");
    }
}

class Zealot extends Unit {
    public void attack() {
        System.out.println("찌르기");
    }

    public void teleportation() {
        System.out.println("프로토스 워프");
    }
}

public class Main {
    public static void main(String[] args) {
    
        Unit unit_up;
        Zealot zealot = new Zealot();
        
        // * 업캐스팅(upcasting)
		unit_up = (Unit) zealot;
		unit_up = zealot; // 업캐스팅은 형변환 괄호 생략 가능
    }
}

////////////////////////////////

unit_up.teleportation(); // COMPILE ERROR

////////////////////////////////

unit_up.attack(); // 오버라이드된 자식 메서드가 실행됨
```
<br>
<br>

**업캐스팅 하는 이유**
- 공통적으로 할 수 있는 부분을 만들어 간단하게 다루기 위해서이다.

- 상속 관계에서 상속 받은 서브 클래스가 몇 개이든 하나의 인스턴스로 묶어서 관리할 수 있기 때문이다.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/aa6bc216-5482-47a3-98b7-30e8181e819d)

- 본래라면 Rectangle, Triangle, Circle 클래스는 서로 다른 타입이니 각각 타입을 정의해서 사용해야 한다.
```javascript
Rectangle[] r = new Rectangle[];
r[0] = new Rectangle();
r[1] = new Rectangle();

Triangle[] t = new Triangle[];
t[0] = new Triangle();
t[1] = new Triangle();

Circle[] c = new Circle[];
c[0] = new Circle();
c[1] = new Circle();
```
<br>

- 하지만 상속 관계를 맺어 부모 클래스로 업캐스팅이 가능하다면, 다음과 같이 하나의 타입으로 묶어 배열을 구성할 수 있게 된다.
```javascript
Shape[] s = new Shape[];
s[0] = new Rectangle();
s[1] = new Rectangle();
s[2] = new Triangle();
s[3] = new Triangle();
s[4] = new Circle();
s[5] = new Circle();
```
<br>

- 주의할 점은 자식 클래스에만 있는 고유한 메서드를 실행을 하지 못한다는 것이다.
<br>
<hr>
<br>

### ✔ DownCasting
- 다운캐스팅은 거꾸로 부모 클래스가 자식 클래스 타입으로 캐스팅 되는 것이다.

- 다운캐스팅은 캐스팅 연산자 괄호를 생략할 수 없다.
  - 멤버의 개수가 적은 것 -> 많은 것으로 변환하기 때문
  - 원시타입 변환때도 작은 데이터 타입이 큰 타입으로 변환할때는 생략할 수 없다.
<br>

- 다운캐스팅의 목적은 업캐스팅한 객체를 다시 자식 클래스 타입의 객체로 되돌리는데 목적을 둔다.
  - 다운 캐스팅은 부모 클래스를 자식클래스로 캐스팅하는 단순히 업캐스팅의 반대 개념이 아니다.
  - 다운 캐스팅의 진정한 의미는 부모 클래스로 업 캐스팅된 자식 클래스를 복구하여, 본인의 필드와 기능을 회복하기 위해 있는 것이다.
  - 즉, 원래 있던 기능을 회복하기 위해 다운캐스팅을 하는 것이다.

```javascript
class Unit {
    public void attack() {
        System.out.println("유닛 공격");
    }
}

class Zealot extends Unit {
    public void attack() {
        System.out.println("찌르기");
    }

    public void teleportation() {
        System.out.println("프로토스 워프");
    }
}

public class Main {
    public static void main(String[] args) {

        Unit unit_up;
        Zealot zealot = new Zealot();

        unit_up = zealot; // 업캐스팅
        
        // * 다운캐스팅(downcasting) - 자식 전용 멤버를 이용하기위해, 이미 업캐스팅한 객체를 되돌릴때 사용
        Zealot unit_down = (Zealot) unit_up; // 캐스팅 연산자는 생략 불가능. 반드시 기재
        unit_down.attack(); // "찌르기"
        unit_down.teleportation(); // "프로토스 워프"
    }
}
```
<br>

- 업캐스팅 된 객체 unit_up 에서 만일 자식 클래스에만 있는 teleportation() 메서드를 실행해야 하는 상황이 온다면,<br>
다운 캐스팅을 통해 자식 클래스 타입으로 회귀 시킨뒤 메서드를 실행하면 된다.

- 만일 메서드를 한번 만 실행 할 것이라 따로 변수에 저장해둘 필요성이 없다면, 아래와 같이 다운 캐스팅을 한줄로 표현할 수도 있다.
```javascript
((Zealot) unit_up).teleportation(); // "프로토스 워프"
```
<br>
<br>

**다운 캐스팅 주의점**
- 앞서 다운 캐스팅의 목적은 업캐스팅한 객체를 되돌리는데 있다고 했다.

- 그래서 다음과 같이 업캐스팅 되지 않는 생 부모 객체 unit 일 경우,<br>
이를 그대로 (Zealot) unit 다운캐스팅 하면 오류(ClassCastException)가 발생하게 된다.

```javascript
Unit unit = new Unit();

// * 다운캐스팅(downcasting) 예외 - 다운캐스팅은 업스캐팅한 객체를 되돌릴때 적용 되는것이지, 오리지날 부모 객체를 자식 객체로 강제 형변환은 불가능
Zealot unit_down2 = (Zealot) unit; //! RUNTIME ERROR - Unit cannot be cast to Zealot
unit_down2.attack(); //! RUNTIME ERROR
unit_down2.teleportation(); //! RUNTIME ERROR
```
<br>

- 💡 위와 같은 다운 캐스팅 특성에 대해 주의해야 할 이유는 에디터에서 컴파일 에러가 발생하기 않고 런타임 에러가 발생하는 위험성이 있기 때문이다.
  - 코드 자체에서는 에러가 뜨지 않지만, 런타임 시에 에러를 발견할 수 있다.
 
![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/0f9911fe-5b2b-4f8d-82f3-50f89cee10c1)
<br>
<hr>
<br>

### ✔ instanceof 연산자
- 참조 캐스팅을 잘못했다가 런타임 환경에서 에러가 나 프로그램이 종료 되버리면 서비스에 크나큰 차질이 생기게 된다.

- 어느 객체 변수가 어느 클래스 타입인지 판별해 true/false를 반환해준다.

- 객체에 대한 클래스(참조형) 타입에만 사용할 수 있다는 점이다. (int, double 같은 primitive 타입에는 사용 불가능)

```javascript
class Unit {
    // ...
}

class Zealot extends Unit {
    // ...
}

public class Main {
    public static void main(String[] args) {

        // * 업캐스팅 유무
        Zealot zealot = new Zealot();

        if (zealot instanceof Unit) {
            System.out.println("업캐스팅 가능"); // 실행
            Unit u = zealot; // 업캐스팅
        } else {
            System.out.println("업캐스팅 불가능");
        }
        
        // * 다운스캐팅 유무
        Unit unit = new Unit();
        Unit unit2 = new Zealot();

        if (unit instanceof Zealot) {
            System.out.println("다운캐스팅 가능");
        } else {
            System.out.println("다운캐스팅 불가능"); // 실행
        }

        if (unit2 instanceof Zealot) {
            System.out.println("다운캐스팅 가능"); // 실행
            Zealot z = (Zealot) unit2; // 다운캐스팅
        } else {
            System.out.println("다운캐스팅 불가능");
        }
    }
}
```
<br>
<hr>
<br>

**Reference**<br>

[자바의 정석 3판 : chapter 07 다형성]<br>
[Inpa Dev : JAVA 업캐스팅 & 다운캐스팅 - 완벽 이해하기](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EC%97%85%EC%BA%90%EC%8A%A4%ED%8C%85-%EB%8B%A4%EC%9A%B4%EC%BA%90%EC%8A%A4%ED%8C%85-%ED%95%9C%EB%B0%A9-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0)
