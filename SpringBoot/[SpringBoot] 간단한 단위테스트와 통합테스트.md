## 단위테스트와 통합테스트
- 매우 간단하게 두 테스트를 진행한다.

- 단위 테스트는 순수 자바로 진행

- 통합 테스트는 스프링 컨테이너를 함께 올려 진행한다.
<br>
<hr>
<br>

### ✔ 단위 테스트
```java
package hello.hellospring.repository;

import hello.hellospring.Repository.MemberRepository;
import hello.hellospring.Repository.MemoryMemberRepository;
import hello.hellospring.domain.Member;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.Test;

import java.util.List;

import static org.assertj.core.api.Assertions.*;

public class MemoryMemberRepositoryTest {

    MemoryMemberRepository repository = new MemoryMemberRepository();

    @AfterEach
    public void afterEach() {
        repository.clearStore();
    }

    @Test
    public void save() {
        //given
        Member member = new Member();
        member.setName("spring");

        //when
        repository.save(member);

        //then
        Member result = repository.findById(member.getId()).get();
        assertThat(member).isEqualTo(result);
    }

    @Test
    public void findByName() {
        Member member1 = new Member();
        member1.setName("spring1");
        repository.save(member1);

        Member member2 = new Member();
        member2.setName("spring2");
        repository.save(member2);

        Member result = repository.findByName("spring1").get();
        assertThat(result).isEqualTo(member1);
    }

    @Test
    public void findAll() {
        Member member1 = new Member();
        member1.setName("spring1");
        repository.save(member1);

        Member member2 = new Member();
        member2.setName("spring2");
        repository.save(member2);

        List<Member> result = repository.findAll();
        assertThat(result.size()).isEqualTo(2);
    }
}

```
<br>

- 순수 자바이기 때문에 객체를 new로 메모리에 등록한다.

- 트랜잭션을 롤백 시킬 수 없기 때문에 @AfterEach를 통해서 각 테스트가 끝날 때 마다 store를 초기화한다.
<br>
<hr>
<br>

### ✔ 통합 테스트
```java
package hello.hellospring.service;

import hello.hellospring.Repository.MemberRepository;
import hello.hellospring.domain.Member;
import org.junit.jupiter.api.Assertions;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.transaction.annotation.Transactional;

import static org.assertj.core.api.Assertions.*;

@SpringBootTest
@Transactional
class MemberServiceTest {

    @Autowired MemberService memberService;
    @Autowired MemberRepository memberRepository;

    @Test
    void join() {
        //given
        Member member = new Member();
        member.setName("spring");

        //when
        Long saveId = memberService.join(member);

        //then
        Member findMember = memberService.findOne(saveId).get();
        assertThat(member.getName()).isEqualTo(findMember.getName());

    }
}
```
<br>

- 통합 테스트이기 때문에 스프링 컨테이너에 올려서 진행한다.
  - `@SpringBootTest` 사용 시 스프링 컨테이너가 테스트와 함께 올라감
<br>

- 각 테스트가 끝나면 `@Transactional`을 사용하여 트랜잭션을 자동으로 롤백 시켜준다.

- MemberService와 MemberRepository는 Bean에 등록되어있기 때문에 `@Autowired`를 사용하여 의존성을 주입한다.
  - 생성자 주입을 하지 않은 이유는 테스트에서는 해당 코드를 다른 곳에서 쓸 가능성이 없기 때문
<br>
<hr>
<br>

### ✔ 공통 사항
- given, when, then 문법을 사용하여 필요한 데이터, 상황, 결과 순으로 작성한다.

- assertThat은 assertions에 있는 함수인데, 해당 객체를 static으로 import 해놓으면 assertions.{함수}() 로 사용하지 않고도 바로 쓸 수 있다.
  - import static org.assertj.core.api.Assertions.*;
  - Assertions가 Junit에서도 있는데 assertj 꺼를 쓰는 것이 더 쓰기 편하다.
<br>
<hr>
<br>

**Reference**<br>

[인프런 김영한 : 스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/dashboard)
