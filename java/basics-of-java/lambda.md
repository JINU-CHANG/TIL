## Lambda Expression (람다식)

람다식은 JDK1.8부터 추가되었다. 람다식의 도입으로 자바는 객체지향언어인 동시에 **함수형 언어**가 되었다.

## 함수형 프로그래밍이란?

Functional Programming(함수형 프로그래밍)은 새로운 프로그래밍 패러다임이다. 

최근의 프로그래밍 패러다임은 크게 아래와 같이 구분할 수 있다.

* 명령형 프로그래밍: 무엇(What)을 할 것인지 나타내기보다 **어떻게(How)** 할 건지를 설명하는 방식
    * 절차지향 프로그래밍: 수행되어야 할 순차적인 처리 과정을 포함하는 방식 (C, C++)
    * 객체지향 프로그래밍: 객체들의 집합으로 프로그램의 상호작용을 표현 (C++, Java, C#)
* 선언형 프로그래밍: 어떻게 할건지(How)를 나타내기보다 **무엇(What)**을 할 건지를 설명하는 방식
    * 함수형 프로그래밍: 순수 함수를 조합하고 소프트웨어를 만드는 방식 (클로저, 하스켈, 리스프)

1~10까지의 값을 출력하는 간단한 예제를 통해 명령형 프로그래밍과 선언형 프로그래밍을 비교해보자.

명령형 프로그래밍에서는 어떻게 할 것인가(How)를 표현했다.
```
// 1 ~ 10까지의 값이 i에 할당된다
for(int i = 1 ; i < 10; i++){
    System.out.println(i);
}
```
다음은 함수형 프로그래밍 코드이다.
```
process(10, (i)->System.out.println(i));
```
함수형 프로그래밍에서는 두번째 인자로 전달받은 값을 출력하라는 함수를 매개변수로 받고 있다. 함수형 프로그래밍은 무엇(What)에 포커스를 두는 프로그래밍 방식이고 따라서 '출력하는 함수'를 파라미터로 넘길 수 있다.

함수형 프로그래밍에서는 함수는 **1급 객체**로 취급하여 파라미터로 넘기는 작업이 가능하다. 1급 객체는 다음과 같은 것들이 가능하다.
* 변수나 데이터 구조 안에 담을 수 있다.
* 파라미터로 전달 할 수 있다.
* 반환값으로 사용할 수 있다.
* 할당에 사용된 이름과 무관하게 고유한 구별이 가능하다.

## 람다식이란?

> 메서드를 하나의 '식'으로 표현한 것

람다식은 함수를 간략하면서도 명확한 식으로 표현할 수 있게 해준다. 메서드를 람다식으로 표현하면 메서드의 이름과 반환값이 없어지므로, 람다식을 'anonymous function(익명함수)'라고도 한다.

익명함수들은 모두 1급 객체이기 때문에 변수처럼 사용가능하며 매개 변수로 전달이 가능하다는 특징을 가지고 있다.

### 람다식 작성하기

람다식은 메서드에서 이름과 반환타입을 제거하고 매개변수 선언부와 몸통{} 사이에 '->'를 추가한다.

- 기존의 작성방식
    ```
    반환타입 메서드이름(매개변수 선언) {
        문장들
    }

    //예시
    int max(int a, int b) {
        return a > b ? a : b;
    }
    ```
- 람다식
    ```
    (매개변수 선언) {
        문장들
    }

    //예시
    (int a, int b) -> {
        return a > b ? a : b;
    }
    ```

## 함수형 인터페이스

자바에서 모든 메서드는 클래스 내에 포함되어야 한다. 람다식은 어떤 클래스에 포함되는 것일까?

람다식은 익명 클래스의 객체와 동등하다. 익명 클래스는 한번 사용되고 버려지는 객체로 이름이 없다.

```
(int a, int b) -> a > b ? a : b

// 위의 람다식을 아래의 익명 클래스로 표현할 수 있다.
new Object() {
    int max(int a, int b) {
        return a > b ? a : b';
    }
}
```
익명 클래스의 메소드를 호출하기 위해서는 참조변수가 필요하고 참조변수의 타입은 참조형이니까 **클래스 또는 인터페이스**가 가능하다.

자바에서는 인터페이스를 통해 람다식을 다루기로 결정했으며, 람다식을 다루기 위한 인터페이스를 'functional interface (함수형 인터페이스)'라고 한다.

다음은 함수형 인터페이스를 사용하는 예시이다.

```
@FunctionalInterface
interface MyFunction { // 함수형 인터페이스 MyFunction 정의
    public abstract int max(int a, int b);
}

// 1번
MyFunction f = new MyFunction() { // 인터페이스를 구현한 익명 클래스
    public int max(int a, int b) {
        return a > b ? a : b;
    };
}

// 2번
MyFunction f = (int a, int b) -> a > b ? a : b ; // 익명 객체를 람다식으로 대체
int big = f.max(5,3);
```

### 함수형 인터페이스 특징

- 오직 **하나의 추상 메서드**만 정의되어야 한다는 제약이 있다. 그래야 람다식과 인터페이스 메서드가 1:1로 연결될 수 있기 때문이다.
- static메서드와 default 메서드 개수에는 제약이 없다.
- @Functional Interface 를 붙여줘야 한다.

## 함수형 인터페이스의 활용

java.util.function 패키지에 일반적으로 자주 쓰이는 형식의 메서드를 함수형 인터페이스로 미리 정의해 놓았다. 매번 새로운 함수형 인터페이스를 정의하지 말고, 가능하면 이 패키지의 인터페이스를 활용하자. 그래야 메서드 이름도 통일되고, 재사용성이나 유지보수 측면에서 좋다.

- 기본적인 함수형 인터페이스
    |함수형 인터페이스|메서드|설명|
    |------|---|---|
    |Supplier<T>|T get()|매개변수는 없고, 반환값만 있다.|
    |Consumer<T>|void accept(T t)|매개변수만 있고, 반환값이 없다.|
    |Function<T,R>|R apply(T t)|일반적인 함수, 하나의 매개변수를 받아서 결과를 반환한다.|
    |Predicate<T>|boolean test(T t)|조건식을 표현하는데 사용된다. 매개변수는 하나, 반환 타입은 boolean이다.|

![예시](https://github.com/JINU-CHANG/TIL/assets/98975580/4fddc4d4-54bb-44e0-a2e9-6b73bf2e8b7e)

### 컬렉션 프레임웍과 함수형 인터페이스

컬렉션 프레임윅의 인터페이스에 다수의 디폴트 메서드가 추가되었는데, 그 중 일부는 위에서 설명한 함수형 인터페이스를 사용한다.

예를 들어, Iterable 인터페이스에 구현된 `void forEach(Consumer<T> action)` 메서드를 보자.

```
List<Integer> list = new ArrayList<>();

for(int i=0; i<10; i++)
    list.add(i);

// list의 모든 요소를 출력
list.forEach(i -> System.out.println(i+","));
```

List의 모든 요소를 출력하기 위해 forEach 메서드에 `람다식(i -> System.out.println(i+","))` 을 전달하였다. 람다식을 전달받아 forEach가 어떻게 동작하는지 아래의 코드에서 살펴보자.

```
default void forEach(Consumer<? super T> action) {
        Objects.requireNonNull(action);
        for (T t : this) {
            action.accept(t);
        }
    }
```

람다식을 매개변수로 받아 Consumer 인터페이스의 accept 메서드를 오버라이딩한 것을 볼 수 있다.

```
Consumer action = new Consumer() {
    public void accept(T t){
        System.out.println(t+",");
    }
}
```

한마디로 람다식은 다음과 같이 익명클래스로 나타낼 수 있으며, forEach 메서드가 실행될 때 for문으로 List의 값을 하나씩 순회하면서 System.out.println(t) 를 실행한다.

## 메서드 참조

람다식이 하나의 메서드만 호출하는 경우 메서드 참조로 람다식을 간략히 할 수 있다.

**static 메서드 참조**

```
List<Integer> list = new ArrayList<>(List.of(1,2,3));
list.forEach(number -> System.out.println(number));

//메서드 참조 적용
List<Integer> list = new ArrayList<>(List.of(1,2,3));
list.forEach(System.out::println);
```

**인스턴스 메서드 참조**

```
posts.stream().map(post -> post.getId()).collect();
posts.stream().map(Post::getId).collect();

//메서드 참조 적용
posts.stream().map(Post::getId).collect();
```

**특정 객체 인스턴스 메서드 참조**

```
MyPrinter my = new MyPrinter();
List<Integer> list = new ArrayList<>(List.of(1,2,3));
list.forEach(number -> my.print(number));

//메서드 참조 적용
MyPrinter my = new MyPrinter();
List<Integer> list = new ArrayList<>(List.of(1,2,3));
list.forEach(my::print);
```

`인스턴스 메서드 참조`와 `특정 객체 인스턴스 메서드 참조`를 구분해야 한다. `인스턴스 메서드 참조`는 람다식 매개변수의 인스턴스 메서드를 참조하는 것이고, `특정 객체 인스턴스 메서드 참조`는 외부 변수의 메서드를 참조하는 것이다.

---
참고
* https://mangkyu.tistory.com/111