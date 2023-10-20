## 오버로딩 (overloading)
- 메서드의 이름이 같지만 배개변수의 개수 또는 타입이 다른 것

- 같은 메서드 이름으로 여러 기능을 구현할 수 있다.
  - 메소드의 이름을 절약할 수 있다.
<br>

- 밑에 예제는 `print()`라는 함수이름은 같지만 모두 매개 변수가 다르다.

```java
class OverloadingMethods {
	public void print() {
		System.out.println("오버로딩1");
	}

	String print(Integer a) {
		System.out.println("오버로딩2");
		return a.toString();
	}

	void print(String a) {
		System.out.println("오버로딩3");
		System.out.println(a);
	}

	String print(Integer a, Integer b) {
		System.out.println("오버로딩4");
		return a.toString() + b.toString();
	}

}
```
<br>
<hr>
<br>

### ✔ 데이터 타입에 따른 오버로딩
```java
int add(int a, int b) {return a+b};
int add(int x, int y) {return x+y};
```

- 매개변수의 이름만 다를 뿐 타입이 같기 때문에 오버로딩이 성립되지 않는다.
<br>
<br>

```java
int add(int a, long b) {return a+b};
int add(long a, int b) {return a+b};
```

- 매개변수가 같지만 데이터 타입이 다르고 그 위치에 따라 추정할 수 있기 때문에 가능하다.
<br>
<hr>
<br>

### ✔ 가변인자와(varargs)와 오버로딩
- 타입...변수명을 통해 매개변수를 다양하게 사용가능하다.

- 아래와 같이 매개변수별로 메서드를 작성하면 낭비가 심하기 때문에<br>
가변인자를 사용하면 하나로 함축이 가능하다.

```java
String concen(String S1, String S2) {return}
String concen(String S1, String S2, String S3) {return}
String concen(String S1, String S2, String S3, String S4) {return}
```

```java
String concen(String... str) {return};
```
<br>

- 가변인자와 다른 매개변수를 같이 쓰려면 가변인자는 제일 마지막에 넣어야 한다.
  - 컴파일러가 확인할 수 없기 때문
  - `String concen(String aa, String... str) {return};`
<br>

**가변인자 오버로딩 예제**
```java
public class VarArgsEx {
    public static void main(String[] args) {
        String[] strArr = {"100", "200", "300"};
        System.out.println(concatenate("", "100", "200", "300"));
        System.out.println(concatenate("-", strArr));
        System.out.println(concatenate(",", new String[]{"1", "2", "3"}));
        System.out.println("["+concatenate(",", new String[0])+ "]");
        System.out.println("["+concatenate(",")+ "]");
    }

    static String concatenate(String delim, String... args) {  // <<<<<<<<<<<<<<<<<< 가변인자를 마지막에 넣는다.
        String result = "";
        for (String str : args) {
            result += str + delim;
        }

        return result;
    }

//
//    static String concatenate(String... args) {
//        return concatenate("", args);
//    }
//
}

///////////// 실행 결과 /////////////////
100200300
100-200-300-
1,2,3,
[]
[]
```
<br>
<hr>
<br>

**Reference**<br>

[자바의 정석 3판 : chapter 06 객체지향 프로그래밍 l 오버로딩]
 
