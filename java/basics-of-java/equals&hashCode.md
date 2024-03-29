## 자바 equals / hashCode 오버라이딩

지금껏 여러번 equals / hashCode가 재정의된 코드를 봤었는데 왜 사용하는지 몰랐고, 내가 사용할 이유가 없었어서 공부하지 않았다.

그러다 문득 `객체의 주소값이 같지 않더라도 특정 필드값이 같으면 같은 객체로 판별하게 할 수 없을까?`라는 고민을 하게되었다.

어렴풋 보았던 equals/hashcode가 이 고민을 해결해줄 수 있을거라 생각했고, 해당 개념을 공부하면서 정리해본다.

## equals 오버라이딩

어떤 두 참조 변수의 값이 같은지 다른지 동등 여부를 비교해야 할 때 사용하는 것이 equals 메서드이다.

클래스 자료형의 객체 데이터일 경우 equals() 메소드는 비교할 대상의 **주소**를 이용하여 비교한다.

그런데, 주소값은 다르지만 특정 필드가 같을 때 같은 객체로 판별하고 싶다면 어떻게 해야 할까?? 예를 들어, Person 객체에서 name필드값이 같다면 같은 객체로 판별하고 싶다면?

이때 equals를 오버라이딩해서 내가 원하는 조건에 따라 같은 객체로 판별되도록 바꿀 수 있다.

```
import java.util.Objects;

class Person {
    String name;

    public Person(String name) {
        this.name = name;
    }

    // 객체 주소 비교가 아닌 Person 객체의 사람 이름이 동등한지 비교로 재정의 하기 위해 오버라이딩
    public boolean equals(Object o) {
        if (this == o) return true; // 현객체와 매개변수의 객체 주소값이 같을 경우 당연히 true 반환
        if (!(o instanceof Person)) return false; // 만일 매개변수 객체가 Person 타입과 호환되지 않으면 false
        Person person = (Person) o; // 만일 매개변수 객체가 Person 타입과 호환된다면 다운캐스팅(down casting) 진행
        return this.name.equals(o.name); // this객체 이름과 매개변수 객체 이름이 같을경우 true, 다를 경우 false
    }
}

```

- 주소값이 같은 경우 당연히 동일 객체로 판단한다.
- 타입이 호환되지 않으면 동일 객체가 아니라고 판단한다.
- 이름값이 서로 같으면 동일 객체로 판단한다.

위와 같은 로직으로 equals를 재구성하였고 이제 name 필드가 같으면 동일 객체로 판단된다.

## hashCode 메소드

처음의 고민은 해결된 듯한데 equals를 오버라이딩할 때 항상 hashCode가 따라붙는 것을 보았다. hashCode는 어떤 이유로 필요한 것일까?

hash 메서드를 재정의하지 않을 시, hash값을 사용하는 CollectionFramework(HashMap, HashSet, HashTable)을 사용할 때 문제가 발생하기 때문이다.

equals만 재정의할 경우 두 객체가 같다고 판단되어도 해시코드 값을 다르게 출력된다.

```
Person p1 = new Person("홍길동");
Person p2 = new Person("홍길동");

//두 객체의 해시 코드
System.out.println(p1.hashCode()); // 460141958
System.out.println(p2.hashCode()); // 1163157884

// 해시코드가 달라도, equals를 재정의 했기 때문에 동등함
System.out.println(p1.equals(p2)); // true
```

두 객체를 중복된 값을 허용하지 않는 리스트인 Set 자료형에 넣어보자. 두 객체가 같은 객체라고 판단해 size가 1이 나올 거라 예상했지만, 예상과 다르게 2가 출력된다.

```
Set<Person> persons = new HashSet<>();
persons.add(p1);
persons.add(p2);

System.out.println(persons.size()); //2가 출력됨.
```

p1과 p2는 논리적으로 같다고 정의했지만 해시코드가 다르기 때문에 중복된 데이터가 컬렉션에 추가된 것이다.

### hashCode와 equals 동작 순서

위와 같이 동작하는 이유는 Collection 이 다음과 같은 과정을 거치기 때문이다.

    hashCode() 리턴값 (true) --> equals() 리턴값 (true) --> 동등객체     
                                            (false) --> 다른 객체
                    (false) --> 다른 객체

hashCode 값을 먼저 비교후 같지 않으면 다른 객체로 판단한다.

따라서 hashCode()를 재정의함으로써 앞서 살펴봤던 문제를 해결할 수 있고 CollectionFramework를 안전하게 사용할 수 있다.
