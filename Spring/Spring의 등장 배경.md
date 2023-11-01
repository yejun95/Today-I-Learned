## Spring의 등장 배경
![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/f4d32f2e-5781-440f-9d90-8fea9221c9c8)

- Spring이 등장하기 이전 EJB (Enterprise JavaBeans)는 애플리케이션의 업무 로직을 담당하는 자바 표준 기술이었다.
  - Java Bean은 자바 객체를 재사용하도록 정의한 것
  - 결국 EJB는 자바 객체를 재사용하도록 정의하여
<br>

- EJB의 등장으로 개발자는 비즈니스 로직에 집중 할 수 있는 환경을 갖추었지만, 하나의 기능을 구현하기 위해 클래스 간 상속,<br>
인터페이스의 구현 등 각 클래스 간의 의존도가 커짐으로 배보다 배꼽이 더 큰 상황에 부딪혔다.
  - 코드의 종속성이 제일 큰 문제
  - 아래와 같이 모든 것이 ejb에 종속되어 있다. 
<br>

```java
import javax.ejb.EJBException;
import javax.ejb.SessionBean;
import javax.ejb.SessionContext;

public class OrdersService implements SessionBean {

    private SessionContext ctx;

    public Orders placeOrder(String menuName) {
        Orders orders = new Orders(menuName);
        orders.init()
        return orders;
    }

    @Override
    public void setSessionContext(SessionContext ctx) throws EJBException {
        this.ctx = ctx;
    }

    @Override
    public void ejbRemove() throws EJBException {

    }

    @Override
    public void ejbActivate() throws EJBException {

    }

    @Override
    public void ejbPassivate() throws EJBException {

    }
}
```

- 이로 인해, 마틴 파울러는 EJB에 반발해 오래된 방식의 "간단한 자바 오브젝트로 돌아가자"는 말을 했고,<br>
이는 POJO(Plain Old Java Object)라는 용어의 기원이 되었다.
<br>
<hr>
<br>

