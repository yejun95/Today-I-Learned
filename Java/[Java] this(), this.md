### ✔ this()
- 생성자에서 자신의 생성자 혹은 다른 생성자를 호출할 때 쓰인다.

- 생성자 간 서로 호출이 가능하다는 뜻이다.

- 한 생성자에서 다른 생성자를 호출할 때 반드시 첫 줄에서만 호출이 가능하다.
<br>

```java
class Car{
    String color;
    String gearType;
    int door; 
    
    Car(){
        this("white", "auto", 4);           // Car(String color, string gearType, int door)를 호출
    }
    
    Car(String color){
        this(color, "auto", 4);
    }
    
    Car(String color, String gearType, int door){
        this.color = color; 
        this.gearType = gearType;
        this.door = door;
    }
}
```

- 밑에 두 타입은 동일하게 초기화 하는 방법이다.
```
Car() {
  color = "white";
  gearType = "auto";
  door = 4;
}

//////////////////////

Car() {
  this("white", "auto", 4);
}
```
<br>
<hr>
<br>

### ✔ this
- 인스턴스 자신을 가르키는 참조 변수이다.

- 아래 두 개 예제는 같은 내용이다.

```java
Car(String c, String g, int d) {
  color = c;
  gearType = g;
  door = d;
}

//////////////////////////

Car(String color, String gearType, int door) {
  this.color = color;
  this.gearType = gearType;
  this.door = door;
}
```
<br>

- 위에 예제에서 `this`는 인스턴스 변수이고, 그 뒤는 생성자의 매개변수이다.
  - 같은 변수명이면 구분이 가지 않기 때문에 `this`를 사용하는 거
  - `this`는 참조변수로 인스턴스로 자신을 가르킨다.
<br>

- `static`은 `this`를 사용할 수 없다.
  - 어차피 인스턴스를 생성하지 않고도 사용가능하기 때문
<br>
<hr>
<br>

**Reference**<br>

[자바의 정석 3판 : chapter 06 this(), this]
