
회원 컨트롤러(MemberController)는 회원서비스(MemberService), 회원 리포지토리(MemberRepository)와 의존관계가 있다.

의존관계는 스프링 컨테이너(Spring Container)가 스프링 빈(Bean)을 등록해서 관리한다.

스프링 컨테이너의 도움 없이 다음과 같이 new 연산자를 통해 객체를 직접 생성할 수 있다.

```
public class MemberController {

    private final MemberService memberService = new MemberService();

}
```

문제는 MemberController뿐 아니라 다른 여러 컨트롤러가 MemberService를 가져다 쓸 수 있고 그때마다 새로 MemberService를 생성해줘야 한다는 것이다. MemberService가 가진 기능은 고정되어 있어서 여러개를 생성할 필요가 없다.

스프링 컨테이너에 MemberService를 등록하면 딱 하나만 등록되면 여러 이점을 볼 수 있다.

```
@Controller
public class MemberController {

    private final MemberService memberService;

    @Autowired
    public MemberController(MemberService memberService) {
        this.memberService = memberService;
    }

}
```

다음과 같이 **@Autowired**가 있으면 스프링이 연관된 객체를 스프링 컨테이너에서 찾아서 넣어준다. 이렇게 객체 의존관계를 외부에서 넣어주는 것을 **DI(Dependency Injection)**, 의존성 주입이라고 한다.

또한 **@Autowired** 가 제대로 작동하기 위해서는 MemberService와 MemberRepository에 **@Service** 어노테이션을 붙여야 한다. 그래야 스프링 컨테이너가 해당 Bean을 자동으로 등록한다. 개발자가 생성한 순수 객체는 스프링 컨테이너가 인식하지 못한다.

### 스프링 빈을 등록하는 2가지 방법

* 컴포넌트 스캔과 자동 의존관계 설정
* 자바 코드로 직접 스프링 빈 등록

<br>

* 스프링은 스프링 컨테이너에 Bean을 등록할 때, 기본으로 싱글톤으로 등록한다(유일하게 하나만 등록해서 공유한다). 따라서 같은 스프링 빈이면 모두 같은 인스턴스다. 설정으로 싱글톤이 아니게 설정할 수 있지만, 특별한 경우를 제외하면 대부분 싱글톤을 사용한다.

### 컴포넌트 스캔 원리

**@Component** 어노테이션이 있으면 Bean으로 자동 등록된다. **@Controller** 컨트롤러가 스프링 빈으로 자동 등록된 이유도 컴포넌트 스캔 때문이다.