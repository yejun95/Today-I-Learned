##  선언위치에 따른 변수의 종류
### ✔ 클래스 변수
- 클래스가 메모리에 올라갈 때 생성된다.
  - JVM이 실행될 때

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/756b8b23-88ee-42f7-ab07-db37b2005733)
<br>

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/7d55cfeb-1d10-472a-87a0-b51a18cc53c0)

<br>

- 모든 인스턴스가 하나의 저장 공간을 공유한다.

- `static`으로 정의한 것들.

- 공통으로 자주 사용할 것들을 해당 영역에 정의한다.
<br>
<br>

### ✔ 인스턴스 변수
- 클래스의 인스턴스를 생성할 때 만들어진다.
  - ex) `Tv v = new Tv()`
<br>
<br>

### ✔ 지역 변수
- 메서드 내 선언되며, 메서드 내에서만 사용 가능하다.
<br>
<br>

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/d3f02cfc-03eb-4198-83d0-ffbb10324634)
<br>
<br>

### ✔ 클래스 변수와, 인스턴스 변수 예시
```java
public class ExampleJAVA {
    public static void main(String[] args) {

        System.out.println("K자동차 최대속도 = " + K_Car.maxSpeed); // 클래스 변수는 객체 생성 없이 사용가능!
        System.out.println("K자동차 제로백= " + K_Car.zero100); // 클래스이름.클래스변수 로 직접 사용한다.

        K_Car k3 = new K_Car();
        k3.model = "22년도k3";
        k3.price = 1700;

        K_Car k5 = new K_Car();
        k5.model = "22년도k5";  //인스턴스 변수의 값을 변경
        k5.price = 2300;

        System.out.println(k3.toString());
        System.out.println(k5.toString());

        k5.maxSpeed = 300; //클래스변수의 값을 변경
        k5.zero100 = 4;

        System.out.println(k3.toString());
        System.out.println(k5.toString());
    }

}

class K_Car{
    String model;
    int price;
    static int maxSpeed = 250;
    static int zero100 = 6;

    @Override
    public String toString() {
        return "K_Car{" +
                "모델='" + model + '\'' +
                ", 가격=" + price + '\'' +
                ", maxSpeed=" + maxSpeed + '\'' +
                ", 제로백=" + zero100 +
                '}';
    }
}
```
![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/b6d87f8b-0da0-41a0-9d3e-7ce6c6ccf080)

- k5의 static 변수를 수정하였지만 `k3.toString()`으로 출력하여도 변경된 값이 나온다.
  - 공통된 영역이기 때문
<br>

### ✔ 변수의 초기화
- 변수를 선언하고 처음으로 값을 저장하는 것을 '초기화'라고 한다.

- 보통 선언과 동시에 초기화를 한다.  ->  명시적 초기화(explicit initialization)
  - `int x = 0;`
<br>

- 멤버변수는 초기화 하지 않아도 되지만, 지역 변수는 반드시 초기화 해야 한다.
  - 멤버 변수는 자동으로 기본값으로 초기화 해준다.
<br>

- 멤버변수는 초기화가 선택이지만 지역변수는 필수다.

```java
class InitTest {
   int x;                  // 인스턴스 변수
   int y = x;              // 인스턴스 변수

   void method1() {
        int i;             // 지역 변수
        int j = i;         // 에러, 지역변수를 초기화하지 않고 사용
  }
}
```
<br>

### ✔ 변수의 초기화 블럭
- 클래스 초기화 블럭, 인스턴스 초기화 블럭 두 가지가 존재한다.
  - 클래스 초기화 블럭은 1번만 실행이 된다.
  - 인스턴스 블럭은 객체 생성 시 마다 실행 된다.
<br>

- 중괄호 `{}`만 사용하여 초기화 내용을 추가한다.

```java
class BlockTest {
	static {
		System.out.println("static { }");
	}

	{
		System.out.println("{ }");
	}

	public BlockTest() {     
		System.out.println("생성자");
	}

	public static void main(String args[]) {
		BlockTest bt = new BlockTest();
                bt.BlockTest()

		BlockTest bt2 = new BlockTest();
                bt2.BlockTest()
	}
}


/////////// 실행 결과 /////////////////////

static{}
{}
생성자
{}
생성자
```
<br>
<br>

**또 다른 예제**
- 인스턴스 초기화 블럭을 통해 count를 증가시킨다.
  - 객체가 생성될 때 마다 실행이 된다.
<br>

- `static`으로 선언됐기 때문에 값이 초기화 되지 않는다.

```java
class Product {
	static int count = 0;   // 생성된 인스턴스의 수를 저장하기 위한 변수
	int serialNo;	        // 인스턴스 고유의 번호

	{
		++count;
		serialNo = count;
	}

	public Product() {}     // 기본생성자, 생략가능
}

class ProductTest {
	public static void main(String args[]) {
		Product p1 = new Product();
		Product p2 = new Product();
		Product p3 = new Product();

		System.out.println("p1의 제품번호(serial no)는 " + p1.serialNo);
		System.out.println("p2의 제품번호(serial no)는 " + p2.serialNo);
		System.out.println("p3의 제품번호(serial no)는 " + p3.serialNo);
		System.out.println("생산된 제품의 수는 모두 "+Product.count+"개 입니다.");  
	}
}

////////////// 실행 결과 ///////////////

p1의 제품번호(serial no)는 1
p1의 제품번호(serial no)는 2
p1의 제품번호(serial no)는 3
생산된 제품의 수는 모두 3개 입니다.
```

<br>
<hr>
<br>

## 메서드
- 특정 작업을 수행하기 위해 어떤 값을 입력하면 이 값으로 작업을 수행하고 결과를 반환한다.

- 사용 이유
  - 재사용성
  - 중복 코드 제거

- 메서드의 구조
  - 매개변수 : 변수들의 타입이 같아도 생략 불가
  - 매개변수도 메서드 내 선언된 것이기에 지역변수다.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/dc82b8af-60f2-46a0-a90c-f92a607e80a5)
<br>
<br>

**메서드 재사용성 예시**
```java
public class Overlap {

    public static void main(String[] args) {
        int[] wesleyArr1 = {1, 2, 3, 4, 5};
        int[] wesleyArr2 = {6, 7, 8, 9, 10};

        printArr(wesleyArr1);
        printArr(wesleyArr2);
    }

    static void printArr(int[] array) {
        for (int num : array) {
            System.out.printf("%d\t", num);
        }
        System.out.println();
    }
}
```
<br>
<br>

**💡 매개 변수의 유효성 검사**
- 메서드를 만들 때 매개 변수의 유효성 검사부터 진행한다.

- 불필요한 함수를 읽지 않기 위함

- 오류 우선 처리(Fail-Fast Principle)

```javascript
float divide(int x, int y) {
  // 작업을 하기 전에 나누는 수 (y)가 0인지 확인한다.
  if(y==0) {
    System.out.println("0으로 나눌 수가 없습니다.");
    return 0;  // 매개변수가 유효하지 않으므로 메서드를 종료한다.
 }
}
```

### ✔ 반환 타입
- 메서드는 실행한 결과 값을 return을 통해 반환해준다. (위쪽 메서드 구조 그림 참조)
  - 반환타입이 `void`인 경우 return 반환값이 없기에 생략 가능하다.<br>
     -> 컴파일러가 자동으로 마지막에 `return;`을 붙여주는 것
  - `void`가 아닌 경우 반드시 return 필요
  - return은 1개만 가능

- 데이터 타입과 return 타입은 반드시 일치해야 한다.
  - 아래 예시를 보면 데이터 타입 -> `int`, 반환 타입 -> `int` 동일하다.

```javascript
int add(int x, int y)
{
   int result = x + y;
   return result;
}
```
<br>

- 조건식의 경우 return을 1개만 넣으면 안된다.
  - ture, false 경우의 수를 생각하여 각각 return을 입력
  - 아래의 경우 b가 더 크면 false이기 때문에 return문이 없다는 에러가 발생
  
```javascript
int max(int a, int b) {
    if(a > b)
      return a;   // 조건식이 참일 때만 실행 
}
```
<br>
<br>

### ✔ 자동 형변환
- 자동으로 타입 변환이 일어나는 것을 의미하며, 자동형변환은 값의 허용 범위가 작은 타입이 허용 범위가 큰 타입으로 저장될 때 발생한다.
  - byte < short/char < int < long < float < double
<br>

- 변수 선언 시 사용되는 기본(primitive) 자료형 데이터타입

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/2940c7eb-a3f9-47af-87e7-169dfcb4bdf7)
<br>
<br>

```java
public class blogTest {
	public static void main(String[] args) {
		byte byteValue = 10;
		int intValue = byteValue; 
		//컴파일시 자동 타입 변환이 되어 byte형 변수가 int형으로 변환되어 대입된다.
		//따라서 int변수가 아닌 다른 타입변수를 대입했는데도 오류가 나지 않음.
		System.out.println(intValue);
		
		long longValue = 25;
		//25는 int타입. 25가 long타입으로 자동 변환되어 long타입으로 저장된다.
		
		System.out.println(longValue);
		
		double doubleValue = 3.14 * 100;
		//실수 연산을 하기 위해 100이 100.0으로 자동형변환되어 계산 된다.
		//다른 피연산자 3.14가 실수이기 때문에
		
		System.out.println(doubleValue);
	}
}
```

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/4f41fd28-a335-46c4-8b3e-b78dabf370e4)
<br>
<br>

### ✔ 강제 형변환
- 큰 허용 범위 타입을 작은 허용 범위 타입으로 강제로 저장하는 것

- 강제로 타입 변환을 지시한다. (()안에 변환할 타입 지정)

```java
public class blogTest {
	public static void main(String[] args) {
		int intValue = 10;
		byte byteValue = (byte)intValue;
		//큰 허용 범위 타입 앞에 (작은 허용범위타입)을 적어 강제 타입 변환을 시킬 수 있다.
		
		System.out.println(byteValue);
		
		intValue = 65;
		char charValue = (char)intValue;
		System.out.println(charValue);
		//int형 65가 char타입으로 변환되면서 유니코드로 변환되어 "A"가 출력된다.
		
		double doubleValue = 1.9;
		intValue = (int)doubleValue;
		System.out.println(intValue);
		//강제 타입 변환으로 소수점 이하 0.9 손실이 발생한다.
	}
}
```
![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/016326de-2871-48f3-9d4e-10c536e01045)
<br>
<hr>
<br>

### ✔ JVM 메모리 구조
[JVM 메모리 구조](https://github.com/yejun95/Today-I-Learned/blob/master/Java/JVM%20%EB%A9%94%EB%AA%A8%EB%A6%AC%20%EA%B5%AC%EC%A1%B0.md)
<br>
<hr>
<br>

## 기본형과 참조형 매개변수
- 기본형 : 변수의 값을 읽기만 가능 -> 메모리 주소 자체가 값으로 되어있다.
  - 이 값이 변경될 경우 예기치 않은 동작이 발생할 수 있다.
  - 따라서 일반적으로 기본형 변수는 값 변경이 불가능하도록 설계되어 있다.
  - 모든 타입은 스택에 저장된다.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/7ffcb346-2747-4662-9ba9-ac4041e09389)
<br>
<br>

- 참조형 : 변수의 값 읽고 변경 가능  ->  값이 저장된 곳의 주소 알기 때문
  - 힙에 실제 값이 저장
  - 스택에서는 힙에 저장된 값의 주소를 저장
  - 배열도 참조형 변수이다.

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/212e8788-1275-4cb1-9338-2d05e7d7ee1e)
<br>
<br>

**스택, 힙의 저장 예시**

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/73e4dc0b-48d2-4b73-9ea5-bbdc8ed1aa18)
<br>
<br>

### ✔ 참조형 반환타입
- 반환타입이 매개변수 뿐만 아니라 참조형 그 자체가 될 수도 있다.

```java
class Data { int x; }

class ReferenceReturnEx {
   public static void main (String[] args) {
	Data d = new Data();
	d.x = 10;

	Data d2 = copy(d);
	System.out.println("d.x = "+d.x);
	System.out.println("d2.x="+d2.x);
 }

 static Data copy(Data d) {
    Data tmp = new Data();
    tmp.x = d.x;

    return tmp;
 }
}

////////////
d.x = 10
d2.x = 10
```
<br>

- 반환 타입이 참조형이라는 것은 메서드가 객체의 주소를 반환한다는 것을 의미
<br>
<hr>
<br>

## 재귀함수 (Recursion Function)
- 메서드 내부에서 메서드 자신을 다시 호출하는 것

```java
void method() {
    method();  // 재귀호출, 메서드 자신을 호출한다.
}
```
<br>

- 그러나 코드상 오로지 재귀호출뿐이면, 자기 자신을 계속 부르기 때문에 무한루프에 빠진다. ->  StackOverflowError 발생

- 때문에 조건식과 함께 사용하여 탈출루트를 만들어 줘야 한다.

- 아래와 같이 if문을 보통사용하며, 여기서 if 문이 빠지면 탈출구가 없기 때문에 똑같이 무한루프에 빠지는 것이다.

**팩토리얼(Factorial) 예제**
```java
int factorial(int n) { 
    if (n === 1) { 
        return 1; 
    } 
    return n * factorial(n-1);
}

/////////// for문 사용

int factorial(int n) {
    int result = 1;
    for (int i = 1; i < n; ++i) {
        result *= i + 1;
    }
    return result;
}

```
![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/a0eaa75c-c66a-4241-8cec-22a397fec18a)
<br>
<br>

- 장점
  - 변수를 여럿 만들 필요가 없다.<br>
    변수가 적어서 편하다는 의미는 변수의 메모리가 적어진다는 의미가 아니라<br>
    변경 가능한 상태인 변수를 줄여 프로그램 오류가 발생할 수 있는 가능성을 줄여준다는 의미
    
  - while문이나 for문같은 반복문을 사용하지 않아도 되기에 코드가 간결해진다.
<br>

- 단점
  - 지속적으로 함수를 호출하게 되면서 지역변수, 매개변수, 반환값을 모두 process stack에 저장해야한다.
  - 이런 과정은 선언한 변수의 값만 사용하는 반복문에 비해서 메모리를 더 많이 사용하게 되고, 이는 속도 저하로 이어진다.
<br>

### ✔ 꼬리 재귀 함수 (tail call recursion)
- 재귀 함수의 가장 큰 문제가 자기 자신을 호출한 뒤 결과를 기다리면서 생기는 콜스택의 부하로 인한 메모리낭비이다.

- 꼬리 재귀라는 개념을 이용하면 재귀 호출이 끝나는 시점에서 아무 일도 하지 않고 바로 결과를 반환하도록 하는 방법으로<br>
함수의 상태 유지 및 추가 연산을 하지 않기에 스택 오버 플로우 해결할 수 있다. 

- 꼬리 재귀의 핵심은 반환부에 연산이 없어야 한다는 점이다

```java
/ Basic Recursion
int factorial(int n, int total) { 
    if (n === 1) { 
        return 1; 
    } 
    return n * factorial(n-1);
}

// Tail Recursion
int factorial(int n, int total) {
    if (n === 1) { 
        return 1; 
    } 
    return factorial(n - 1, n * total);
}
```

```
초기 호출: factorial(5, 1) (n=5, total=1)
첫 번째 재귀 호출: factorial(4, 5 * 1) (n=4, total=5)
두 번째 재귀 호출: factorial(3, 4 * 5 * 1) (n=3, total=20)
세 번째 재귀 호출: factorial(2, 3 * 4 * 5 * 1) (n=2, total=60)
네 번째 재귀 호출: factorial(1, 2 * 3 * 4 * 5 * 1) (n=1, total=120)
```
<br>
<br>

**오버플로우 문제**
```java

class A {
	public void paint() {
		System.out.print("A");
		draw();
	}
	public void draw() {
		System.out.print("B");
		draw();
	}
}

class B extends A {
	public void paint() {
		super.draw();
		System.out.print("C");
		this.draw();
	}
	public void draw() {
		System.out.print("D");
	}
}

public class Gisafirst {
	public static void main(String[] args) {
		A b = new B();
		b.paint();
		b.draw();
	}
}
```
<br>
<hr>
<br>

## 클래스와 인스턴스의 메서드, 변수
- static이 붙으면 클래스, 그게 아니라면 인스턴스다.

- 클래스 메서드와 변수는 어디서에서나 사용가능이 가능한 공통의 영역이다.

- 인스턴스 메서드와 변수는 인스턴스 객체를 만들어야지만 사용이 가능하다.
<br>
<br>

### ✔ 사용 가능 여부
- 클래스 메서드와 변수는 누구나 사용 가능하다.

- 클래스 메서드와 변수는 인스턴스 변수를 사용할 수 없다.
  - 생성되는 영역도 상이하다. (클래스 영역, 힙 영역)
<br>

![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/8877d467-682b-4664-894c-67d15be71bc7)
<br>
<hr>
<br>

**Reference**<br>

[자바의 정석 3판 : chapter 06 변수와 메서드]<br>
[olivejua : [WS live-study ] 1주차:JVM은 무엇이며 자바 코드는 어떻게 실행하는 것인가](https://olivejua-develop.tistory.com/46)<br>
[쿠쿠의 개발일지 : 클래스변수, 인스턴스변수, 지역변수 - JAVA](https://dding9code.tistory.com/51)<br>
[자바비터 : JAVA 변수/자동형변환/강제형변환/캐스팅/프로모션](https://javabeater.tistory.com/7)<br>
[Inpa dev : JAVA 변수의 기본형 & 참조형 타입 차이 이해하기](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EB%B3%80%EC%88%98%EC%9D%98-%EA%B8%B0%EB%B3%B8%ED%98%95-%EC%B0%B8%EC%A1%B0%ED%98%95-%ED%83%80%EC%9E%85#%EA%B8%B0%EB%B3%B8%ED%98%95_%ED%83%80%EC%9E%85_primitive_type)<br>
[Catsbi's DLog : 재귀함수의 장점과 단점 그리고 해결책](https://catsbi.oopy.io/dbcc8c79-4600-4655-b2e2-b76eb7309e60)<br>


