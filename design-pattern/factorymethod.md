## 팩토리 메서드 패턴

### 🤔 요구사항
- Client는 Ship 제작을 요청한다.
- Ship가 만들어지는 과정은 다음과 같다.
    - name, email 값이 null이 아닌지 확인한다.
    - '준비중'이라는 메시지를 출력한다.
    - 생성된 배의 정보를 출력한다.

### 문제의 코드

Ship이 만들어지는 과정을 ShipFactory 클래스의 orderShip()으로 구현하였다.

```
public static Ship orderShip(String name, String email) {
        // validate
        if (name == null || name.isBlank()) {
            throw new IllegalArgumentException("배 이름을 지어주세요.");
        }
        if (email == null || email.isBlank()) {
            throw new IllegalArgumentException("연락처를 남겨주세요.");
        }

        prepareFor(name);

        Ship ship = new Ship();
        ship.setName(name);

        // Customizing for specific name
        if (name.equalsIgnoreCase("whiteship")) {
            ship.setLogo("\uD83D\uDEE5️");
        } else if (name.equalsIgnoreCase("blackship")) {
            ship.setLogo("⚓");
        }

        // coloring
        if (name.equalsIgnoreCase("whiteship")) {
            ship.setColor("whiteship");
        } else if (name.equalsIgnoreCase("blackship")) {
            ship.setColor("black");
        }

        // notify
        sendEmailTo(email, ship);

        return ship;
    }
```

해당 코드는 새로운 배가 추가될 때마다 이름과, 색을 지정하는 부분의 **if문을 추가**해야 한다는 문제가 있다.

기능이 확장될 때마다 if문을 수정해야 하는 일은 번거로운 작업이다. 이를 해결하기 위해 공통되는 작업을 추상화하고 구현 클래스에서 구체적인 작업을 구현하도록 할 수 있다.

이를 확장에는 열려있고, 변경에는 닫혀 있는 설계라고 하며 객체 지향의 중요한 원칙 중 하나이다.

++ 추가로, 추상화를 적용했더니 더 많은 클래스들이 생성됐다. 과연 이 방식이 if문의 번거로움을 해결해주는 방법인가 ..?

일단 if 분기문이 많이 늘어나면 한눈에 기능을 파악하기 어렵다는 문제가 있다. if 분기문을 사용하지 않으면 한눈에 보고도 해당 메서드가 무슨 기능을 하는지 알 수 있으면 코드가 더 깔끔해진다는 장점이 있다. 또한 기존 코드를 변경하는 것은 상당한 위험이 있는 작업이다. 변경의 영향으로 제대로 동작하던 기능이 동작하지 않을 수도 있다. 따라서 변경은 최소화해야 하는데, 이러한 이유 때문에 분기문을 작성하는 코드가 좋지 못한 코드라고 할 수 있을 것 같다.

### 설계 변경

어떤 배를 만들던지 **비슷한 패턴(템플릿)**으로 만들어지고 일부분만 변경된다는 사실에 주목하자.

ShipFactory의 orderShip()을 아래와 같이 5단계로 추상화할 수 있다.

1. validate(name, email);
2. prepareFor(name);
3. Ship ship = createShip();
4. sendEmailTo(email, ship);
5. return ship;

추상화된 메서드들을 인터페이스 내부에 구현하고 구체 클래스에서 이를 구현하도록 하면 변경에 닫혀있는 코드를 만들 수 있다.

### 팩토리 메서드 패턴 적용

ShipFactory 인터페이스를 생성한다.

```
public interface ShipFactory {

    default Ship orderShip(String name, String email) {
        validate(name, email);
        prepareFor(name);
        Ship ship = createShip();
        sendEmailTo(email, ship);
        return ship;
    }

    private void sendEmailTo(String email, Ship ship) {
        System.out.println(ship.getName() + " 다 만들었습니다.");
    }

    Ship createShip(); //구현해야 할 부분

    private void validate(String name, String email) {
        if (name == null || name.isBlank()) {
            throw new IllegalArgumentException("배 이름을 지어주세요.");
        }
        if (email == null || email.isBlank()) {
            throw new IllegalArgumentException("연락처를 남겨주세요.");
        }
    }

    private void prepareFor(String name) {
        System.out.println(name + " 만들 준비 중");
    }

}

```

- default 메서드
    - 원래 기존의 인터페이스는 추상 메서드만 존재할 수 있고, 이를 상속받는 구현체에서 직접 해당 메서드를 구현해야 한다.
    - 이는 기능이 확장되었을 때 인터페이스를 구현하는 모든 클래스의 코드를 변경해야 하는 문제점을 가져온다.
    - 디폴트 메서드를 사용하면 인터페이스 내부에서도 로직 구현이 가능한데 이를 통해 기존 구현을 고치지 않고도 인터페이스를 바꿀 수 있다. (추상 클래스와 비슷해진 느낌!)

- private 메서드
    - 인터페이스 내부에서만 사용한다. default 메서드나 static 메서드에서 사용하며 구현 메소드에서 재정의 및 사용이 불가하다.


다음과 같이 ShipFactory 클래스를 구현해 필요한 부분만 구현할 수 있다.

```
public class WhiteshipFactory implements ShipFactory {

    @Override
    public Ship createShip() {
        return new Whiteship();
    }
}


public class BlackshipFactory implements ShipFactory {
    @Override
    public Ship createShip() {
        return new Blackship();
    }
}


```