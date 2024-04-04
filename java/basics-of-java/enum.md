## Enum 등장 배경과 장점(feat. 상수를 정의하는 방법)

### 1. final 상수
* ```private final static int``` 형식으로 작성할 수 있다.
* static을 사용하여 메모리에 한 번만 할당되게 설정된다.
* 접근제어자들 때문에 가독성이 좋지 못하다는 단점이 존재한다.

### 2. 인터페이스 상수
* 인터페이스 내에 상수를 선언할 수 있다.
* public static final 속성을 생략할 수 있어 코드를 조금 더 간결하게 작성할 수 있다.
* 하지만 문제는 다른 집합에 저의된 상수끼리 비교하는 로직이 가능하거나, 잘못된 상수가 할당되었음에도 결국은 정수값이기 때문에 컴파일 에러없이 실행된다는 점이다.
* 예를 들어, ```FRUIT.APPLE == COMPANY.APPLE``` 두 개는 다른 의미임에도 정수값이 같아 같다고 판단된다.

### 3. 자체 클래스 상수
* 상수를 고유의 객체로 선언하자는 취지로 자체 클래스 인스턴스화를 이용해 상수처럼 사용하는 기법이 등장했다.
* 하지만 큰 문제는 switch 문에서 사용할 수 없다는 단점이 있다.
* switch 문의 조건에 들어가는 데이터 타입이 제한적이기 때문이다.

### 4. Enum
* 이런 문제들 때문에 자바에서는 아예 상수만을 다루는 enum 타입 클래스를 만들어 배포했다. 

## Enum에서 ==과 equals 차이

**==사용**

컴파일 타임에 타입체크가 가능하다. 또한 NPE를 피할 수 있다.

```java

public Color getOtherColor() {

    if (this == Color.BLACK) { // this == PieceType.BISHOP이면 에러!
        return Color.WHITE;
    }
    
    return Color.WHITE;
}

```

**equals사용**

런타임에 타입체크가 되며 null이 입력되면 NPE가 발생한다.

```java

public Color getOtherColor() {

    if (this.equals(Color.BLACK)) { // this == PieceType.BISHOP이어도 오케이!
        return Color.WHITE;
    }
    
    return Color.WHITE;
}

```

### 그렇다면 null이 들어오면 NPE가 맞는가? false가 맞는가?



- https://heepie.me/32
- https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EC%97%B4%EA%B1%B0%ED%98%95Enum-%ED%83%80%EC%9E%85-%EB%AC%B8%EB%B2%95-%ED%99%9C%EC%9A%A9-%EC%A0%95%EB%A6%AC#%EA%B3%BC%EA%B1%B0%EC%97%94_%EC%83%81%EC%88%98%EB%A5%BC_%EC%96%B4%EB%96%BB%EA%B2%8C_%EC%A0%95%EC%9D%98%ED%96%88%EB%8A%94%EA%B0%80?
- https://lazypazy.tistory.com/278