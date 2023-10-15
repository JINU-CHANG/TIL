## DI(의존 관계 주입/의존성 주입)

* 의존성은 의존대상 B가 변하면, 그것이 A에 영향을 미친다는 뜻이다.
* 의존성 주입은 객체를 직접 생성하는 것이 아니라(예를 들어 new 연산자를 사용하는 것) **외부에서 생성한 후 주입**시켜 주는 방법이다.
* 의존관계 주입을 사용하면 클라이언트 코드를 변경하지 않고, 클라이언트가 호출하는 대상의 타입 인스턴스를 변경할 수 있다.
* 의존관계 주입을 사용하면 정적인 클래스 의존관계를 변경하지 않고, 동적인 객체 인스턴스 의존관계를 쉽게 변경할 수 있다.

## 스프링과 DI

회원 컨트롤러(MemberController)는 회원서비스(MemberService), 회원 리포지토리(MemberRepository)와 의존관계가 있다.

의존관계는 **스프링 컨테이너(Spring Container)**가 **스프링 빈(Bean)**을 등록해서 관리한다.

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

위의 코드 어디에도 MemberService 클래스의 인스턴스를 생성하지 않았다. 하지만 **@Autowired** 어노테이션을 사용하여 별도의 코드를 작성하지 않고도 Controller에 MemberService의 인스턴스가 주입되었다.

다음과 같이 **@Autowired**가 있으면 스프링이 연관된 객체를 스프링 컨테이너에서 찾아서 넣어준다. 이렇게 객체 의존관계를 외부에서 넣어주는 것을 **DI(Dependency Injection)**, 의존성 주입이라고 한다.

또한 **@Autowired** 가 제대로 작동하기 위해서는 MemberService와 MemberRepository에 **@Service** 어노테이션을 붙여야 한다. 그래야 스프링 컨테이너가 해당 Bean을 자동으로 등록한다. 개발자가 생성한 순수 객체는 스프링 컨테이너가 인식하지 못한다.

### 스프링 빈을 등록하는 2가지 방법

* 컴포넌트 스캔과 자동 의존관계 설정
* 자바 코드로 직접 스프링 빈 등록

<br>

* 스프링은 스프링 컨테이너에 Bean을 등록할 때, 기본으로 싱글톤으로 등록한다(유일하게 하나만 등록해서 공유한다). 따라서 같은 스프링 빈이면 모두 같은 인스턴스다. 설정으로 싱글톤이 아니게 설정할 수 있지만, 특별한 경우를 제외하면 대부분 싱글톤을 사용한다.

### 컴포넌트 스캔 원리

**@Component** 어노테이션이 있으면 Bean으로 자동 등록된다. **@Controller** 컨트롤러가 스프링 빈으로 자동 등록된 이유도 컴포넌트 스캔 때문이다.


## IoC(제어의 역전)

* IoC와 DI는 밀접한 관련이 있다. IoC가 더 상위 개념으로, DI는 IoC 원칙을 실현하기 위한 여러 디자인 패턴 중 하나이다. 
    * = 스프링의 의존성 주입 방법을 사용하므로써 의존성 제어권을 프레임워크로 역전시킬 수 있다.
* 스프링 컨테이너가 필요에 따라 개발자 대신 bean들을 관리해주는 행위
* 개발자가 직접 new 연산자를 통해 객체를 생성하고, 객체의 의존성을 맺어주는 게 아니라, 스프링 컨테이너에 Bean을 등록하기만 하면, 스프링 컨테이너가 Bean의 생명주기(생성-> 의존성 설정 -> 초기화 -> 소멸)을 전부 관리해준다.

### IoC의 장점
* 개발자는 객체 관리에 덜 신경쓸 수 있게 되어 개발 로직에 집중할 수 있다.
* 약한 결합을 이용해 객체 간 의존 관계를 쉽게 변경할 수 있다.

* 참고
    * https://velog.io/@damiano1027/Spring-%EC%9D%98%EC%A1%B4%EC%84%B1-%EC%A3%BC%EC%9E%85-%EC%A0%9C%EC%96%B4%EC%9D%98-%EC%97%AD%EC%A0%84
    * https://velog.io/@ohzzi/Spring-DIIoC-IoC-DI-%EA%B7%B8%EA%B2%8C-%EB%AD%94%EB%8D%B0
    * https://velog.io/@ohzzi/Spring-DIIoC-%EC%8A%A4%ED%94%84%EB%A7%81%EC%9D%98-%EC%9D%98%EC%A1%B4%EC%84%B1-%EC%A3%BC%EC%9E%85