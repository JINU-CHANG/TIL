## AOP(Aspect Oriented Programming)란 ?

Aspect Oriented(관점 지향)은 어떤 로직을 기준으로 **핵심적인 관점**, **부가적인 관점**으로 나누어서 보고 그 관점을 기준으로 모듈화하겠다는 것이다.

* 모듈화 : 어떤 공통된 로직이나 기능을 하나의 단위로 묶는 것

핵심적인 관점은 비즈니스 로직이 될 수 있고, 부가적인 관점은 핵심 로직을 실행하기 위해 행해지는 데이터베이스 연결, 로깅, 파일 입출력 등이 될 수 있다.

### AOP는 흩어진 관심사(Crosscutting Concerns)를 모듈화 할 수 있는 프로그래밍 기법이다.

모든 메서드의 호출 시간을 측정하고자 코드를 추가하는 예시를 보자. 메서드가 1000개라면 1000개의 메서드에 다음과 같은 코드를 추가해야 한다.

```
 /**
* 회원가입
*/
    public Long join(Member member) {
        long start = System.currentTimeMillis();
try {
validateDuplicateMember(member); //중복 회원 검증
            memberRepository.save(member);
            return member.getId();
        } finally {
            long finish = System.currentTimeMillis();
            long timeMs = finish - start;
            System.out.println("join " + timeMs + "ms");
} }
```

모든 메서드에 시간을 측정하는 로직이 추가될 것이고 이런 식으로 소스 코드상에서 계속 반복해서 사용되는 부분들을 흩어진 관심사(CrossCutting Concerns)라고 한다.

이렇게 작성된 코드는 핵심 비즈니스 로직과 섞여 유지보수를 어렵게 만들고, 그렇다고 별도의 공통 로직으로 만들기도 매우 어렵다. 그리고 시간을 측정하는 로직이 변경된다면 모든 로직을 찾아가면서 변경해야 한다는 문제점이 있다.

AOP는 흩어진 관심사를 모듈화하고, 모듈화 시킨 것을 어디에 적용시킬지만 정의해주면 된다. 이때 모듈화 시켜놓은 블록을 **Aspect**이라고 한다.

## 스프링 AOP

* 스프링에서 제공하는 AOP는 프락시(Proxy)기반의 AOP 구현체이다.

* 스프링 AOP는 스프링 Bean에만 적용할 수 있다.

* 모든 AOP 기능을 제공하는 것이 목적이 아닌, 중복 코드, 프록시 클래스 작성의 번거로움 등 흔한 문제를 해결하기 위한 솔루션을 제공하는 것이 목적이다.

Proxy 과정 추가 공부 필요 !

