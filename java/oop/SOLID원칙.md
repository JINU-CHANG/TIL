## 좋은 객체 지향 설계의 5가지 원칙(SOLID)

## 개방-폐쇄 원칙(OCP)

> 소프트웨어 개체는 확장에 대해 열려 있어야 하고, 수정에 대해서는 닫혀 있어야 한다.

- 확장에 대해 열려 있다
    - 애플리케이션의 요구사항이 변경될 때 이 변경에 맞게 새로운 '동작'을 추가해서 애플리케이션의 기능을 확장할 수 있다.
- 수정에 대해 닫혀 있다
    - 기존 '코드'를 수정하지 않고도 애플리케이션의 동작을 추가하거나 변경할 수 있다.

OCP의 핵심은 **추상화**에 의존하는 것이다. 

**추상화**는 핵심적인 부분만 남기고 불필요한 부분은 생략함으로써 복잡성을 극복하는 기법이다. 공통적인 부분은 문맥이 바뀌더라도 변하지 않아야 한다. 이러한 공통적인 부분을 추상화함으로써 문맥이 바뀌더라도 수정에 대해 닫혀있다. 또한 추상화를 통해 생략된 부분은 확장의 여지를 남기며 확장을 가능하게 한다.

## 리스코프 치환원칙(LSP)

LSP는 올바른 상속 관계의 특징을 정의한다.

> 서브 타입(자식)은 언제나 기반 타입(부모)으로 교체할 수 있어야 한다. 

즉 자식 클래스는 부모 클래스가 하는 모든 행동을 동일하게 할 수 있어야 한다는 의미이다. 이를 어떤 타입이 다른 타입의 서브타입이 되기 위해서는 **행동 호환성**을 만족시켜야 한다고 말한다.

다형성을 이용한 코드가 올바르게 동작하는 것을 보장하기 위한 원칙이라고 생각할 수 있다.

리스코프 치환원칙을 지키지 않는 코드는 어떤 문제가 있을까?

- 닭은 새이다(Chicken is-a Bird).
- 새는 날 수 있다.

다음의 가정하에 코드를 짜면 다음과 같다.

```java
public class Bird {
    ...
    public void fly() {...}
}

public class Chicken extends Bird {
    ...
    @Override
    public void fly() {
        throw new UnsupportedOperationException(); 
    }
}
```

```java
public void flyBird(Bird bird) {
    // 인자로 전달된 모든 bird는 날 수 있어야 한다.
    bird.fly();
}
```

클라이언트 입장에서 flyBird를 실행시키면 제대로 메서드가 동작할 것이라고 기대한다. 하지만 닭은 날 수 없다. 따라서 Bird를 상속받은 Chicken클래스는 fly() 메서드에 대해 예외를 던진다. 클라이언트 관점에서 Chicken과 Bird의 행동이 호환되지 않는 것이다. 예상치 못한 예외를 던지는 이 설계는 올바르지 못하다.

또한 리스코프 치환 원칙은 개방-폐쇄 원칙을 만족하기 위한 전제조건이라고 말한다. 클라이언트 관점에서 자식 클래스가 부모 클래스를 대체할 수 있다면 기능 확장을 위해 자식 클래스를 추가하더라도 코드를 수정할 필요가 없어진다.

## 인터페이스 분리 원칙(ISP)

> 범용적인 인터페이스보다 클라이언트가 실제로 사용하는 인터페이스를 만들자(인터페이스를 사용에 맞게끔 각기 분리해야 한다.)

인터페이스의 추상 메서드를 범용적으로 구현하다보면 그 인터페이스를 상속받은 클래스는 자신이 사용하지 않는 인터페이스를 억지로 구현할 수도 있다. 또한 사용하지도 않는 인터페이스의 추상메서드가 변경되면 클래스에서도 수정이 필요하다. ISP 원칙은 클라이언트의 기대에 따라 인터페이스를 분리함으로써 변경에 의한 영향을 제어하는 설계 원칙이다.

인터페이스로 스마트폰을 추상화해보자. 요즘 스마트폰이 가지고 있을 기능을 다채롭게 포함하고 있다.

```java
interface ISmartPhone {
    void call(String number); // 통화 기능
    void message(String number, String text); // 문제 메세지 전송 기능
    void wirelessCharge(); // 무선 충전 기능
    void AR(); // 증강 현실(AR) 기능
    void biometrics(); // 생체 인식 기능
}
```

여기서 갤럭시 S23 클래스를 구현한다면 최신 스마트폰인 이 객체는 ISP 원칙을 만족한다. 하지만 **구형 기종 스마트폰 클래스**를 다뤄야 할 경우 문제가 생긴다. 

구형 기종에는 무선 충전, 생채 인식과 같은 최신 기능이 포함되어 있지 않다. 다음과 같이 구현하는 경우 이를 다루는 클라이언트 입장에서는 ISmartPhone 클래스의 인스턴스들이 일부 기능을 지원하지 않는다는 점에 당황스러울 것이다. 올바르지 않은 설계이다.

```java
class S3 implements ISmartPhone {
    public void call(String number) {
    }

    public void message(String number, String text) {
    }

    public void wirelessCharge() {
        System.out.println("지원 하지 않는 기능 입니다.");
    }

    public void AR() {
        System.out.println("지원 하지 않는 기능 입니다.");
    }

    public void biometrics() {
        System.out.println("지원 하지 않는 기능 입니다.");
    }
}
```

이는 각각의 기능에 맞게 인터페이스를 잘게 분리하여 해결할 수 있다.

```java
interface IPhone {
    void call(String number); // 통화 기능
    void message(String number, String text); // 문제 메세지 전송 기능
}

interface WirelessChargable {
    void wirelessCharge(); // 무선 충전 기능
}

interface ARable {
    void AR(); // 증강 현실(AR) 기능
}

interface Biometricsable {
    void biometrics(); // 생체 인식 기능
}
```

```java
class S3 implements IPhone {
    public void call(String number) {
    }

    public void message(String number, String text) {
    }
}
```

S3는 필요한 기능에 맞게 인터페이스를 구현함으로써 더 이상 필요하지 않는 기능을 추가로 구현하지 않아도 된다.