## 대표적인 Spring 디자인 패턴의 종류
### ✔ Singleton pattern
- 어플리케이션당 오직 하나의 인스턴스만 존재하도록 보장해주는 패턴이다.

- 싱글턴 패턴(Singleton pattern)을 따르는 클래스는, 생성자가 여러 차례 호출되더라도 실제로 생성되는 객체는 하나이고 최초 생성 이후에 호출된 생성자는 최초의 생성자가 생성한 객체를 리턴한다
<br>

**Singleton Beans**
-  일반적으로 Singleton Object는 어플리케이션에서 글로벌하게 유일해야하지만, Spring에서는 이러한 제약이 완화된다.

-  Spring Framework가 하나의 Application Context당 하나의 Bean을 생성하는 것을 의미한다. 

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/326a1e9d-c88a-41f7-97c8-70d22928540f)
<br>
<br>

**Autowired Singletons**
- 기본적으로, Spring Framework는 모든 Bean들을 Singleton으로 생성한다.

- Spring Controller에서 Autowired 하는 것이 바로 싱글톤 인스턴스를 생성하는 것이다.

- ex) 단일 Application Context 내에서 두 Controller를 생성하고, 같은 타입의 Bean을 각각에 주입할 수 있다.

- @Autowired : Spring Container에서 관리중인 Bean/자료형을 자동 맵핑해주는 어노테이션.
  - 최초 어플리케이션 구동시 Bean을 컨테이너에 등록하고 @Autowired 구문을 찾아 해당 변수에 등록한 Bean을 주입하여 사용한다.

```java
@RestController
public class LibraryController {
    
    @Autowired
    private BookRepository repository;

    @GetMapping("/count")
    public Long findCount() {
        System.out.println(repository);
        return repository.count();
    }
}
```

```java
@RestController
public class BookController {
     
    @Autowired
    private BookRepository repository;
 
    @GetMapping("/book/{id}")
    public Book findById(@PathVariable long id) {
        System.out.println(repository);
        return repository.findById(id).get();
    }
}
```
<br>
<hr>
<br>

### ✔ Factory Method pattern
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/72445008-a727-4f93-9f74-ced6cdd41f6c)

- 원하는 객체를 생성하기 위해 추상 메서드가 있는 팩토리 클래스를 생성한다.
  - 추상 클래스 혹은 interface가 된다. 

-  자식(하위) 클래스가 어떤 객체를 생성할지를 결정하도록 하는 패턴이기도 하다.
  - 추상 클래스 상속 및 인터페이스를 구현하여 메서드를 오버라이드하는 개념이다.
<br>

- 각 객체를 추상 클래스 및 interface로 관리 할 수 있다는 장점이 있다.
  - 공통 코드 무결성 보장
<br>
<hr>
<br>

### ✔ Proxy pattern
![image](https://github.com/BJSNuruhee/levelup/assets/121341413/1b7c5318-8477-4d63-9dd6-bb069f47538f)

- 일반적으로 프록시는 다른 무언가와 이어지는 인터페이스의 역할을 하는 클래스이다.

- 하나의 개체(프록시)가 다른 개체(주체 또는 서비스)에 대한 액세스를 제어할 수 있도록 하는 패턴이다.
  - ex) 사장님한테 다이렉트로 가는게 아니라 비서한테 먼저 물어보는 개념
  - ex) 용량이 큰 이미지와 글이 같이 있는 문서를 모니터 화면에 띄운다고 가정하였을때 이미지 파일은 용량이 크고 텍스트는 용량이 작아서 텍스트는 빠르게 나타나지만 그림은 조금 느리게 로딩되는 된다. 그러므로 먼저 로딩이 되는 텍스트라도 먼저 나오게 하기 위하여 텍스트 처리용 프로세서, 그림 처리용 프로세스를 별도로 운영한다.
 <br>

**장점**
- 사이즈가 큰 객체(ex : 이미지)가 로딩되기 전에도 프록시를 통해 참조를 할 수 있다.

- 실제 객체의 public, protected 메소드들을 숨기고 인터페이스를 통해 노출시킬 수 있다. 
<br>

**단점**
- 객체를 생성할 때 한 단계를 거치게 되므로, 빈번한 객체 생성이 필요한 경우 성능이 저하될 수 있다.

- 로직이 난해해져 가독성이 떨어질 수 있다.
<br>
<br>

**예제 소스**

Image.java(interface)
```java
public interface Image {
   void displayImage();
}
```
<br>
<br>

Real_Image.java
```java
public class Real_Image implements Image {

    private String fileName;
    
    public Real_Image(String fileName) {
        this.fileName = fileName;
        loadFromDisk(fileName);
    }
    
    private void loadFromDisk(String fileName) {
        System.out.println("Loading " + fileName);
    }
    
    @Override
    public void displayImage() {
        System.out.println("Displaying " + fileName);
    }
}
```
<br>
<br>

Proxy_Image.java
```java
public class Proxy_Image implements Image {
    private Real_Image realImage;
    private String fileName;
    
    public Proxy_Image(String fileName) {
        this.fileName = fileName;
    }
    
    @Override
    public void displayImage() {
        if (realImage == null) {
            realImage = new Real_Image(fileName);
        }
        realImage.displayImage();
    }
}

```
<br>
<br>

Proxy_Main.java
```java
public class Proxy_Main {
    public static void main(String[] args) {
        Image image1 = new Proxy_Image("test1.png");
        Image image2 = new Proxy_Image("test2.png");
        
        image1.displayImage();
        System.out.println();
        image2.displayImage();
    }
}
```
<br>
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/98f27313-1d3f-4897-b3c6-6297829e34c3)

- 위의 예제를 보면 Proxy_Main에서 Real_Image에 직접 접근하지 않고 Proxy_Image 객체를 생성하여 대신 시킨다.

-  Proxy는 displayImage() 메서드를 호출하고 그 반환값을 Main에 반환한다.
<br>
<hr>
<br>

### ✔ Template Method Pattern
- 어떤 작업을 처리하는 일부분을 서브 클래스로 캡슐화해 전체 일을 수행하는 구조는 바꾸지 않으면서 특정 단계에서 수행하는 내역을 바꾸는 패턴

- 디자인 패턴이라고 하기도 뭐할정도로 객체지향 언어로 개발을 하다보면 무의식적으로 사용하는 패턴이다.

**장점**
- 중복코드를 줄일 수 있다.

- 자식 클래스의 역할을 줄여 핵심 로직의 관리가 용이하다.
<br>
<br>

**단점**
- 추상 메소드가 많아지면서 클래스 관리가 복잡해진다.

- 클래스간의 관계와 코드가 꼬여버릴 염려가 있다.
<br>

**예제 소스**
```java
//추상 클래스 선생님
abstract class Teacher{
	
    public void start_class() {
        inside();
        attendance();
        teach();
        outside();
    }
	
    // 공통 메서드
    public void inside() {
        System.out.println("선생님이 강의실로 들어옵니다.");
    }
    
    public void attendance() {
        System.out.println("선생님이 출석을 부릅니다.");
    }
    
    public void outside() {
        System.out.println("선생님이 강의실을 나갑니다.");
    }
    
    // 추상 메서드
    abstract void teach();
}
 
// 국어 선생님
class Korean_Teacher extends Teacher{
    
    @Override
    public void teach() {
        System.out.println("선생님이 국어를 수업합니다.");
    }
}
 
//수학 선생님
class Math_Teacher extends Teacher{

    @Override
    public void teach() {
        System.out.println("선생님이 수학을 수업합니다.");
    }
}

//영어 선생님
class English_Teacher extends Teacher{

    @Override
    public void teach() {
        System.out.println("선생님이 영어를 수업합니다.");
    }
}

public class Main {
    public static void main(String[] args) {
        Korean_Teacher kr = new Korean_Teacher(); //국어
        Math_Teacher mt = new Math_Teacher(); //수학
        English_Teacher en = new English_Teacher(); //영어
        
        kr.start_class();
        System.out.println("----------------------------");
        mt.start_class();
        System.out.println("----------------------------");
        en.start_class();
    }
}

```


![image](https://github.com/BJSNuruhee/levelup/assets/121341413/8d2ab131-07c7-4dd9-bd94-6527632883c2)
<br>
<hr>
<br>

**Reference**<br>

[carrots-day :  Spring.02.디자인 패턴 ](https://carrots-day.tistory.com/20)<br>
[newtownboy : proxy pattern](https://velog.io/@newtownboy/%EB%94%94%EC%9E%90%EC%9D%B8%ED%8C%A8%ED%84%B4-%ED%94%84%EB%A1%9D%EC%8B%9C%ED%8C%A8%ED%84%B4Proxy-Pattern)


