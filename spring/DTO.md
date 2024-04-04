## DTO(Data Transfer Object)

계층 간 데이터 전송을 위해 도메인 모델 대신 사용되는 객체이다.

DTO는 순수하게 데이터를 저장하고, 데이터에 대한 getter/setter만을 가져야 한다. DTO는 어떠한 비즈니스 로직을 가져서는 안 되며, 저장, 검색, 직렬화, 역직렬화 로직만을 가져야 한다고 한다.

여기서 **비즈니스 로직**은 현실 문제에 대한 의사결정을 하는 코드를 뜻한다.

- 그 결정을 위한 입력값인가?
- 그 결정의 결과물을 해석하고 보여주고 전파하는 코드인가?

위의 질문을 만족하면 비즈니스 로직이 아니다.

## Domain 대신 DTO를 사용하는 이유

계층 간 데이터를 도메인 객체가 아닌 DTO를 사용해서 넘겨주는 이유는 도메인 객체를 안전하게 보호하기 위해서이다. 다른 계층으로 도메인 객체를 넘겨주면 도메인 데이터가 변경될 수도 있고, 또한 굳이 사용되지 않는 정보까지 다른 계층으로 넘길 필요가 없다.

그리고 모델과 뷰의 강한 결합을 끊을 수 있다. 

```java
// Controller

Car car = new Car("name");

printCar(car.getName(), car.getPosition());

// OutputView

public void printCar(final String name, final int position) {
    .....
}

```

다음의 코드에서 뷰의 요구사항 변화로 출력해야 하는 데이터가 추가된다고 가정해보자. 그렇다면 Controller, OutputView 두 곳에서 수정사항이 발생한다. 이때 DTO를 사용하면 이 결합을 느슨하게 할 수 있다.

```java
// Controller

Car car = new Car("name");

printCar(car.getName(), car.getPosition(), car.getColor());

// OutputView

public void printCar(final String name, final int position, final String color) {
    .....
}

```

## Domain과 DTO가 헷갈린다?

### 👋🏻 질문

>  Domain은 비즈니스 로직을 수행하는 클래스이고 DTO는 계층간의 데이터 전달을 위해 사용된다고 알고 있습니다. 하지만 막상 코드를 구현하다보니 비즈니스 로직인가? 단순히 데이터 전달을 위한 값인가?로 둘을 확실히 구분하기 힘든 클래스들도 많았습니다. Scroes나 CommandDto도 마찬가지이구요. 코드를 짜면서 저만의 기준을 만드는 것이 어려운데 이에 대해 조언해주실만한 부분이 있다면 궁금합니다 🙂

### 👍 답변

> 도메인일까요 DTO일까요? 클래스를 만들고 나서 이건 도메인, 이건 DTO 구분을 짓는 건 의미가 없는 것 같아요. 제제가 처음 클래스를 만든 의도에 따라 내용을 채워서 제제의 의도를 잘 반영해보세요. 도메인을 의도한 클래스라면 Domain은 비즈니스 로직을 수행하는 클래스라는 제제의 생각에 따라 도메인의 역할에 맞게 내용을 채워주면 되겠죠.
이 Scores는 도메인을 의도하고 만든 것 같은데 지금은 값을 담고만 있을 뿐 Scores가 하는 역할이 전혀 없네요. 도메인을 풍부하게 하려면 어떤 방법이 있을까요? 점수를 계산하는 로직을 Scores로 옮겨봐도 괜찮을 것 같아요.

처음 클래스를 만든 의도에 따라 내용을 채워 의도를 반영해보자!

### 체스 미션에서 점수는 DTO인가 도메인인가?

점수는 누가 계산하나 ?  
-> 점수를 계산하기 위해서는 `살아있는 기물들`과 `기물들의 위치`가 필요  
-> ChessBoard가 가지고 있는 정보

ChessBoard가 처리하게 할 것인가? 계산하는 클래스를 분리할 것인가?  
-> 계산로직이 복잡하고 크기 때문에 클래스로 분리하는게 깔끔하다.  
-> ScoreCalculator

ScoreCalculator가 계산해서 반환하는 값은 DTO인가 Domain인가?  
-> 이 객체가 하는 역할은 blackScore, whiteScore의 데이터를 가지고 View로 전달하는 것  
-> 어떤 의사결정을 하는 코드인가? 아니다. 단순히 데이터만 저장하고 있을 뿐이다. 결국 Scores는 DTO인것.
