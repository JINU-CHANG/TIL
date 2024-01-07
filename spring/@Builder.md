## 빌더 패턴

빌더 패턴은 다음의 이유로 사용한다.

- 가독성
    - 생성자로 객체를 생성하는 경우 변수가 많아지면 어떤 데이터인지 분별하는 것이 쉽지 않지만 빌더는 어떤 변수에 어떤 값이 들어가있는지 파악하기 편리하다.
- 유연성
    - 파라미터의 전달 순서가 굉장히 중요한 생성자와 달리 빌더는 순서를 고려하지 않아도 된다. 또한 변수를 추가하거나 삭제할 때도 생성자보다 유연하다.

### 사용 방법

### 1. ```@Builder```, ```@AllArgsConstructor```, ```@NoArgsConstructor``` 사용

```
@Builder
@AllArgsConstructor
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class User {
	private String name;
    private String gender;
    private String phone;
    private int age;
}
```

@Builder, @NoArgsConstructor(access = AccessLevel.PROTECTED) 를 사용하는 경우 반드시 @AllArgsConstructor를 붙여줘야 한다.

@Builder는 생성자 유무에 따라 다음과 같이 동작한다.

- 생성자가 없는 경우 : 모든 멤버 변수를 파라미터로 받는 기본 생성자 생성
- 생성자가 있을 경우 : 따로 생성자를 생성하지 않음

@NoArgsConstructor(access = AccessLevel.PROTECTED) 어노테이션에 의해 기본 생성자가 존재하기 때문에 @Builder는 생성자를 별도로 생성하지 않는다. 하지만 이 기본 생성자에는 접근 제한 속성이 부여되어 있기 때문에 문제가 발생한다. 따라서 모든 필드를 파라미터로 가지는 @AllArgsConstructor 어노테이션이 필요하다.

### 2. 생성자에 @Builder 설정

```
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class User {
	private String name;
    private String gender;
    private String phone;
    private int age;

    @Builder
    public User(String name, String gender, String phone, int age) {
        this.name = name;
        this.gender = gender;
        this.phone = phone;
        this.age = age;
    }
}
```

다음과 같이 생성자에 어노테이션을 붙일 수도 있다.