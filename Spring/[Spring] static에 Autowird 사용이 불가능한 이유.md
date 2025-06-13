# static에 Autowird 사용이 불가능한 이유
- static 클래스나 필드는 객체가 아닌 클래스 수준에서 존재한다.

- Spring은 객체 인스턴스를 기준으로 DI를 수행하므로 static 대상에는 DI가 불가능하다.

- `@Autowired`는 Spring 컨테이너가 관리하는 빈 인스턴스의 필드에 의존성을 주입하는 반면, static 변수나 메서드는 클래스 레벨에서 존재하며, 특정 인스턴스와 연결되지 않는다.

- Spring의 빈은 애플리케이션 컨텍스트에 의해 관리되며, 그 생명주기는 애플리케이션 컨텍스트에 의해 제어되는 반면 static 필드는 클래스 로딩 시점에 초기화되며, 애플리케이션 컨텍스트와는 별개로 존재한다.

- 때문에 @Autowired된 빈을 static 클래스나 필드에 직접 사용할 수 없다.
<br>
<hr>
<br>

## ✔️ 코드
- IDE : IntelliJ

```java
@Service
public class MemberService {

    @Autowired
    private MemberRepository memberRepository;

    public void testStaticClassAccess() {
        StaticInnerClass.doSomething();
    }

    public static class StaticInnerClass {

        @Autowired
        private MemberRepository staticRepo;

        public static void doSomething() {
            System.out.println("staticRepo = " + staticRepo);
        }
    }
}
```
<br>

- 위 코드는 흔하게 볼 수 있는 Service에서의 Repository를 주입받는 


