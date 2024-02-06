## public static void main(string args)란 무엇인가?
- main함수는 자바 프로그램의 시작점이다.

- 자바가상머신(JVM)은 main이라는 이름을 가진 메서드를 찾아 프로그램을 시작한다.

- public : JVM이 main함수를 찾을 수 있도록 한다.

- static : JVM이 main함수를 곧바로 실행할 수 있도록 한다.

- void : main함수가 종료되면 프로그램도 종료되므로, return값이 필요하지 않다.
  
- String[] args : 커맨드라인 등을 통해, main함수 내부에서 사용할 수 있는 String 데이터를 전달할 수 있다.
<br>
<hr>
<br>

### ✔ JVM의 main 메서드 탐색과 실행
- 자바 프로그램은 JVM을 통해 실행되고, `main`이라는 이름이 붙은 메서드를 찾아 프로그램을 시작하도록 설계되어 있다.

- 이 때 JVM이 메서드를 올바르게 식별할 수 있도록 `main` 메서드에는 알맞은 제어자(modifier)가 붙어 있어야 한다.
<br>
<br>

 **자유로운 접근을 허용하는 public**
- 접근 제한이 없고, 자바 프로젝트 내에서 자유롭게 접근할 수 있다.
  - default(기본값, 접근 제어자를 명시하지 않음) : 같은 패키지 내에서만 접근 가능.
  - private : 같은 클래스 내에서만 접근 가능.
  - protected : 같은 패키지 + 다른 패키지의 자식 클래스에서 접근 가능.
  - public : 접근 제한 없음.
<br>

**JVM과 main 메서드**
- JVM이 이제 막 Java 프로그램을 시작하고 main 메서드를 실행하려 하는 시점은 아직 어떤 클래스도 로드(load)되어 있지 않은 상태이다.

- 가장 먼저 main 메서드가 어떤 클래스에 있는지 찾아서, 해당하는 클래스를 찾아서 불러오는 과정이 필요하다.

- 이 때 만약에 main 함수에 아무 접근 제어자도 달려 있지 않거나(default), private 또는 protected가 붙어 있다면,<br>
'클래스 내부에서만 접근을 허용한다, 패키지 내부에서만 접근을 허용한다'등등 접근 제약이 생기게 될 것이다.

- 그러면 JVM은 접근 제약 조건을 통과하지 못해 main 메서드를 찾지 못하므로 프로그램 시작이 되지 않는다.

- 💡 위에 사유 때문에 main 메소드에 아무런 접근 제약이 존재하지 않도록 public을 붙여주어야 하는 것이다.
<br>
<hr>
<br>

### ✔ 인스턴스 생성이 필요없는 static
- 일반적으로 클래스에 선언한 속성이나 메서드를 사용하기 위해서는 먼저 해당 클래스의 인스턴스를 생성한 다음 접근해야 한다.

```java
Example e = new Example();
//Example의 새 인스턴스를 생성하고, 참조변수 e가 해당 인스턴스를 가리킴

e.pub();
//참조변수 e를 통해 Example 클래스의 pub() 메서드에 접근함
```
<br>
<br>

- 하지만 속성이나 메서드를 static으로 선언한다면 클래스의 인스턴스를 생성하지 않더라도 호출할 수 있다는 특성을 갖는다.
  - [JVM 메모리 구조](https://github.com/yejun95/Today-I-Learned/blob/master/Java/JVM%20%EB%A9%94%EB%AA%A8%EB%A6%AC%20%EA%B5%AC%EC%A1%B0.md)
<br>

**main 메서드의 경우**
- JVM이 Java 프로젝트 내에서 main 메서드를 발견하면, main 메서드가 들어있는 클래스를 로드하게 된다.

- 그 후 따로 인스턴스를 생성하는 과정을 거치지 않고, 클래스가 로드되면 곧바로 main메서드를 사용할 수 있도록 static을 붙여주어야 한다.
<br>
<hr>
<br>

### ✔ return값이 없는 반환 타입 void
- return문은 메서드의 실행이 종료되었을 때, 메서드의 실행 결과를 전달해주는 역할을 수행한다.
<br>
<br>

**main 메서드의 경우**
- 다른 메서드들은 return 값이 있을 수도, 없을 수도 있다.

- 하지만 main 메서드의 경우 모든 메서드가 실행을 마치면 main 메서드도 종료가 되어야 하는데,<br>
main 메서드의 종료는 곧 프로그램의 종료이고, 더 이상 실행할 메서드도 없는데 maim 메서드가 결과값을 같는다면<br>
그것을 사용해 볼 기회조차 없을 것이다.

- 결국 main 메서드는 반환값이 필요하지 않기 때문에, 아무런 반환값을 return하지 않도록 반환 타입을 void로 선언하는 것이다.
<br>
<hr>
<br>

### ✔ 매개변수 String[] args
- 매개변수는 메서드가 기능을 수행하기 위해 필요한 값(인자, argument)들을 받아오는 변수이다.

- main 메서드도 마찬가지로, args라는 이름을 가진 String 배열 타입의 매개변수를 가지며,<br>
여러 개의 문자열을 입력받아 프로그래머가 지정한 기능을 수행할 수 있습니다.
  - 반드시 매개변수명을 args로 쓸 필요는 없으며, 때때로 'argv'라고 쓰는 프로그래머도 있다.
  - String[]은 String...으로 대체할 수 있으며 같은 의미이다. (아주 오래된 Java 버전에서는 불가)
<br>

- 하지만 실제로 Java 프로그램을 만들면서 이 args라는 매개변수를 사용해 본 적이 있는가?
  - 요즘같이 IntelliJ나 Eclipse같은 IDE로 Java를 공부하는 시대에는 main 메서드에 문자열을 전달할 일이 정말 손에 꼽을 정도로 드물 것이다.
<br>

- 사실 이건 Java가 DOS환경에서 실행되던 시절의 흔적이다.
  - 다음 예제를 통해 명령 프롬프트로 Java 코드를 실행하여, 문자열 인자를 main 메서드에 전달해 보도록 하겠다.
<br>
<br>

**예제 코드**
```java
package test;

import java.util.Arrays;

public class Example {

	public static void main(String[] args) {
		System.out.println("총 " + args.length + "개의 값을 입력받았습니다.");
        //전달받은 배열 인자가 가진 String 갯수를 출력합니다.
        
		System.out.println("입력 내용 : " + Arrays.toString(args));
        //전달받은 배열 인자의 내용을 출력합니다.
	}
	
}
```
<br>
<br>

**명령 프롬프트로 실행**
- 컴파일된 .class 파일을 실행시킨다.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/2d67fcb1-59f5-4232-b045-d0125be96e98)
<br>
<br>

- * java -classpath . Example aa bb cc
  - Example.class를 실행하고, main함수에 aa, bb, cc를 String 배열 형태로 전달한다.
<br>

- 실행결과
```
총 3개의 값을 입력받았습니다.
입력 내용 : [aa, bb, cc]
```
<br>

- 보는 바와 같이 main 메서드가 전달받는다는 String 배열은, 바로 사용자가 커맨드라인에 입력하는 문자열들을 가리키는 것이다.

- 커맨드라인에 입력한 문자열들(aa, bb, cc)의 갯수와 그 내용이 출력된 것을 확인할 수 있다.
<br>
<hr>
<br>

**Reference**<br>

[AtomicLiquors : [Java] public static void main(String[] args)는 무슨 뜻인가요?](https://atomicliquors.tistory.com/7)<br>
