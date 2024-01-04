## 프로토타입(Prototype) 패턴

실제 제품을 만들기 앞서 테스트를 위한 샘플 제품을 만드는데 이때 샘플 제품을 **프로토타입**이라고 칭한다.

프로토타입패턴은 인스턴스를 생성하는데 비용이 많이 들고, 비슷한 객체가 이미 있는 경우 사용되는 생성 패턴 중 하나이다. DB 혹은 네트워크 요청을 거쳐 가져온 데이터를 기반으로 인스턴스를 만드는 경우를 예로 들 수 있다. 이때 프로토타입패턴은 기존 인스턴스를 복제하여 새로운 인스턴스를 만드는 메커니즘을 제공한다.

인스턴스를 복제하기 위해서는 ```자바의 clone```을 이용한다.

### 프로토타입 구현

githubIssue를 clone해서 githubIssue2를 만들고자 한다.

```
GithubRepository repository = new GithubRepository();
repository.setUser("whiteship");
repository.setName("live-study");

GithubIssue githubIssue = new GithubIssue(repository);
githubIssue.setId(1);
githubIssue.setTitle("1주차 과제: JVM은 무엇이며 자바 코드는 어떻게 실행하는 것인가.");

GithubIssue githubIssue2 = githubIssue.clone(); //to-be
```

해당 코드만으로는 compile error가 발생한다. clone 메서드를 사용하기 위해서는 다음이 구현되어 있어야 한다.

- 복제할 클래스가 Cloneable 인퍼에시를 구현해야 한다.
- 복제할 클래스가 Object의 clone 메서드를 재정의(override)하여 접근 제어자를 public으로 변경해야 한다. Object의 clone 메서드의 접근제어자는 protected로 지정되어있기 때문에 재정의하지 않으면 접근할 수 없다.

-> 왜 protected로 지정되어 있는걸까??

```
public class GithubIssue implements Cloneable {
    private int id;

    private String title;

    private GithubRepository repository;

    ...

    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
```

clone하면 다음을 만족한다.

```
System.out.println(clone != githubIssue); //서로 다른 주소값을 가짐
System.out.println(clone.equals(githubIssue)); //equals를 재정의하여 서로 다른 주소값을 가지더라도 내용이 같다면 equals 만족
System.out.println(clone.getClass() == githubIssue.getClass()); //서로 같은 타입
```

그런데, 강의에서는 분명 얕은 복사라고 했는데 어떻게 서로 다른 주소값을 가지지 ?

### 얕은 복사와 깊은 복사

**얕은 복사**

자바의 객체와 같은 참조변수는 직접 값을 저장하는게 아닌 힙(heap) 영역에 데이터를 저장하고 주소값을 저장한다. 객체와 같은 참조형(reference) 타입의 변수를 그대로 복제하면 그 값이 복사되는 것이 아닌 주소값이 복사되어 결국 같은 힙의 데이터를 바라보게 된다. 이러한 복사를 '얕은 복사(shallow copy)'라고 한다. 얕은 복사에서는 원본을 변경하려면 복사본도 영향을 받는다.

**깊은 복사**

반면 깊은 복사(deep copy)는 원본이 참조하고 있는 힙의 데이터까지 복제하는 것을 말하며 깊은 복사에서는 원본과 복사본이 서로 다른 객체를 참조하기 때문에 원본의 변경이 복사본에 영향을 미치지 않는다.

-> 얕은 복사이면 ```System.out.println(clone == githubIssue);```를 만족시켜야하는 것 아닌가 하는 의문이 생겼다.

### clone 메소드

강의에서 말하는 얕은 복사는 위의 개념과 살짝 다른 의미로 언급된 듯하다.

clone()은 단순히 객체에 저장된 값을 그대로 복제할 뿐, ```객체가 참조하고 있는 객체까지 복제하지는 않는다.``` 참조하는 있는 객체에 초점을 두고 얕은 복사라고 설명한다.

![KakaoTalk_Photo_2024-01-04-22-35-47](https://github.com/JINU-CHANG/TIL/assets/98975580/531754c9-2094-432d-8370-47203155b070)

```System.out.println(clone.getRepository() == githubIssue.getRepository());```

이 결과는 어떻게 나올까?

clone이 얕은 복사를 사용하기 때문에 서로 같은 주소값을 가져 true값이 출력된다.

깊은 복사를 하고 싶다면 다음과 같이 clone 메서드를 재정의하면 된다.

```
 @Override
protected Object clone() throws CloneNotSupportedException {
    GithubRepository repository = new GithubRepository();
    repository.setUser(this.repository.getUser());
    repository.setName(this.repository.getName());

    GithubIssue githubIssue = new GithubIssue(repository);
    githubIssue.setId(this.id);
    githubIssue.setTitle(this.title);

    return githubIssue;
}
```

### 프로토타입패턴의 장점과 단점

**장점**
- 복잡한 객체를 만드는 과정을 숨길 수 있다.
- 기존 객체를 복제하는 과정이 새 인스턴스를 만드는 것보다 비용(시간 또는 메모리)적인 면에서 효율적일 수도 있다.
- 추상적인 타입을 리턴할 수 있다.

-> 메모리적으로 어떤 부분이 효율적인 것일까 ? 얕은 복사를 통해 동일한 인스턴스를 참조하고 있어 메모리면에서 효율적이라는 말인가 ? 만약 이게 맞다면 얕은 복사의 단점을 그대로 떠안는 굉장히 위험한 부분이라고 생각되는데

**단점**
- 복제한 객체를 만드는 과정 자체가 복잡할 수 있다. (특히, 순환 참조가 있는 경우)

