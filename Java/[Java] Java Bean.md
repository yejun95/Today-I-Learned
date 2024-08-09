## Java Bean이란?
- 클래스를 표현하는 하나의 규칙

- 데이터를 표현하기 위한 목적을 지닌다.

- 이러한 규칙을 지닌 클래스를 Java Bean 이라고 한다.
  - private로 구성되어 getter와 setter를 통해서만 접근
<br>

- JavaBean은 데이터를 표현하는 것을 목적으로 하는 자바 클래스로서 아래와 같은 형태.
  - JavaBean 규약에 따르는 클래스를 자바 빈 이라고 부르며, jsp에서 사용되는 자바 빈은 아래와 같은 형태.
  - Java Bean은 쉽게 말해 MVC 패턴에서 데이터를 표현해주는 Model에서 사용하기 위한 표현의 형태.
  - jsp에서는 <Jsp:useBean> 태그를 이용한 자바 객체를 사용한다.
  - `<jsp:useBean id = "[이름]" class="[자바빈클래스이름]" scope="[범위]"/>`
<br>

- DTO 혹은 VO의 형태
<br>

```java
public class JavaBean_Test {

    private String id;
    private String password;
    private String email;
    private String name;
    private String address;

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }
}
```
<br>
<hr>
<br>

### ✔ 사용 이유
- 일관적인 데이터 사용
  - 규칙이 있는 클래스라고 설명이 되어 있는데, 이는 정해진 데이터를 알맞게 사용해야 되기 때문이다.
  - ex) 데이터베이스를 통해 회사원의 정보를 조회, 수정할 때, 사원id, name, 직책 등을 그때마다 생성해서 사용하면 효율이 떨어진다.
<br>

- 공통된 정보를 관리하는 기능에서 편리하게 사용하기 위해 규칙성 있는 형태로 데이터의 집합을 만든 것이라고 생각.
<br>
<hr>
<br>

**Reference**<br>

[coreminw.log : Java Bean이란?](https://velog.io/@coreminw/Java-Bean%EC%9D%B4%EB%9E%80)<br>
