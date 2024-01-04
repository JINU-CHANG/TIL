## 빌더패턴

TourPlan 객체를 생성하고 Setter 메소드를 통해 값들을 세팅해주는 코드이다. 
Setter 메소드를 사용함으로써 유연한 객체 생성이 가능해졌지만 문제점이 있다.

```java
TourPlan tourPlan = new TourPlan();

tourPlan.setTitle("칸쿤 여행");
tourPlan.setNights(2);
tourPlan.setDays(3);
tourPlan.setStartDate(LocalDate.of(2020, 12, 9));
tourPlan.setWhereToStay("리조트");

tourPlan.addPlan(0, "체크인 이후 짐풀기");
tourPlan.addPlan(0, "저녁 식사");
tourPlan.addPlan(1, "조식 부페에서 식사");
tourPlan.addPlan(1, "해변가 산책");
tourPlan.addPlan(1, "점심은 수영장 근처 음식점에서 먹기");
tourPlan.addPlan(1, "리조트 수영장에서 놀기");
tourPlan.addPlan(1, "저녁은 BBQ 식당에서 스테이크");
tourPlan.addPlan(2, "조식 부페에서 식사");
tourPlan.addPlan(2, "체크아웃");
```

- 불완전한 객체를 생성한다.
    - Nights와 Days는 반드시 둘 다 설정해줘야하는 값이다. 그런데 개발자가 깜빡하고 하나를 설정해주지 않는다면 유효하지 않는 객체가 생성된다. 이를 다른 곳에서 사용하다가 런타임 예외가 발생할 수 있다.
- 불변성이 보장되지 않는다.
    - Setter 메소드를 통해 언제든 객체의 상태를 변경가능하다. 협업 과정에서 다른 개발자가 어디에서 Setter 메서드를 호출할지 모른다.

(위와 같이 Setter 메소드를 이용해 클래스 필드의 초기값을 설정해주는 방식을 **자바 빈(Java Bean) 패턴**이라고도 한다.)

### 빌더 패턴

```java
public interface TourPlanBuilder {

    TourPlanBuilder nightsAndDays(int nights, int days);

    TourPlanBuilder title(String title);

    TourPlanBuilder startDate(LocalDate localDate);

    TourPlanBuilder whereToStay(String whereToStay);

    TourPlanBuilder addPlan(int day, String plan);

    TourPlan getPlan();

}
```

- 메소드의 리턴 타입을 `TourPlanBuilder`로 두는 이유
    - 다른 메소드들을 **체이닝 방식**으로 호출해줌으로써 자연스럽게 인스턴스를 구성하고, 마지막에 getPlan()를 통해 최종적으로 생성된 객체를 리턴하도록 한다.

빌더 패턴은 위의 문제점을 해결한다.

- nightsAndDays() 메서드로 필수 파라미터를 강제했다.
- 더 이상 Setter 메서드로 객체값을 함부로 변경하지 못한다.

정리하자면 빌더 패턴을 사용하면 다음과 같은 장점이 있다.

- 객체 생성 안정성 확보
- 불변성 확보
- 가독성 향상
    
    ```java
    // 생성자 방식
    Student student1 = new Student(2016120091, "홍길동", 20, "freshman");
    
    -> 각각의 데이터가 무엇을 의미하는지 코드만 봐서는 파악하기 힘들다.
    
    // 빌더 방식
    Student student2 = new StudentBuilder()
                .id(2016120091)
                .name("임꺽정")
    						.age(28)
                .grade("Senior")
                .build();
    -> 빌더 패턴을 적용하면 직관적으로 어떤 데이터에 어떤 값이 있는지 한눈에 파악하기 쉬워진다.
    ```
    
- 유연성 확보
    
    ```java
    //생성자 방식
    Student student1 = new Student(2016120091, "홍길동", "freshman", "010-5555-5555");
    Student student2 = new Student(2016120091, "홍길동", "freshman");
    
    -> 이를 가능하게 하기 위해서는 생성자를 2개 지정해줘야 한다.
    
    //빌더 방식
    Student student1 = new StudentBuilder()
                .id(2016120091)
                .name("임꺽정")
                .grade("Senior")
                .phoneNumber("010-5555-5555")
                .build();
    Student student2 = new StudentBuilder()
                .id(2016120091)
                .name("임꺽정")
                .grade("Senior")
                .build();
    
    -> 동일한 Builder 클래스로 각각 다르게 구성된 객체를 만들 수 있다.
    ```
    

### Builder 디자인 패턴 종류

빌더 패턴은 2가지 종류가 존재한다. **이펙티브 자바의 빌더패턴(디렉터 빌더 패턴)** 과 **GoF의 빌더 패턴(심플 빌더 패턴)** 이다. 

Builder를 통해 객체를 생성한다는 점에서 전체적인 맥락은 비슷하다. GoF 빌더 패턴은 강의에서 설명한 디렉터 빌더 패턴으로 디렉터 클래스를 통해 객체를 다양하게 구현하고 싶을 때 사용한다. 반면 이펙티브 자바는 static inner class 빌더를 사용하는 패턴으로 하나의 대상 객체 생성만을 위해 사용하기 위해 사용한다.

### 빌더 패턴 응용 사례

- Lombok의 @Builder
    - @Builder 를 붙여주면 클래스를 컴파일 할 때 자동으로 클래스 내부에 빌더 API가 만들어진다. @Builder는 심플 빌더 패턴을 다룬다.

- StringBuilder
    - 빌더에 해당하는 StringBuilder를 생성하고, 빌더가 제공하는 append 메서드로 파라미터를 구성하고, 최종적으로 toString을 호출해서 String 객체를 생성
        ```java
        String result = new StringBuilder()
                    .append("hello ")
                    .append("world!")
                    .toString(); // build()
        ```
    

* 참고
    * [https://inpa.tistory.com/entry/GOF-💠-빌더Builder-패턴-끝판왕-정리#빌더_패턴_탄생_배경](https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%EB%B9%8C%EB%8D%94Builder-%ED%8C%A8%ED%84%B4-%EB%81%9D%ED%8C%90%EC%99%95-%EC%A0%95%EB%A6%AC#%EB%B9%8C%EB%8D%94_%ED%8C%A8%ED%84%B4_%ED%83%84%EC%83%9D_%EB%B0%B0%EA%B2%BD)