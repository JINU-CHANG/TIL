## 싱글톤 패턴

- 인스턴스를 오직 한 개만 제공하는 클래스

### 인스턴스가 한 개만 필요한 이유

### 싱글톤 패턴 구현 1

```
public class Settings {
    private static Settings instance; 
    
    private Settings() { }
    
    public static Settings getInstance() { 
        if (instance == null) {
            instance = new Settings(); 
        }

        return instance; 
    }
}
```

- 생성자를 private으로 만든 이유?
    - 하나의 인스턴스만 생성하기 위해서는 생성자로 무분별하게 객체가 생성되는 것을 막아야 한다. 따라서 생성자를 private 으로 설정하고 객체 생성은 static 메서드 내부에서 한다.

- getInstance() 메소드를 static으로 선언한 이유?
    - 인스턴스 하나를 모두 공유하기 위해서는 인스턴스에 관계없이 동일한 값을 가지는 static 변수로 만들어야 한다.
    - 이러한 static 변수에 접근하기 위해서 getInstance() 메서드는 static으로 선언될 필요가 있다.
    - 또한 객체가 생성되기 전에 instance가 생성되었는지 판단할 필요가 있으므로, getInstance() 메서드는 객체 생성없이도 사용이 가능한 static 메서드가 되어야 한다고 말할 수 있다.

- getInstance()가 **멀티쓰레드 환경**에서 안전하지 않은 이유?
    - 스레드 A, 스레드 B가 존재한다고 가정한다.
    - 스레드 A가 if문을 평가하고 인스턴스 생성 코드로 진입한다.
    - 이때 스레드 B가 if문을 평가한다. 아직 스레드 A가 인스턴스화 코드를 실행을 안시켰기 때문에 이 if문도 참이 되게 된다.
    - 결과적으로 스레드 A와 스레드 B가 인스턴스화 코드를 2번 실행시켜 서로 다른 객체를 생성하게 된다.

### 싱글톤 구현 패턴 2

> 동기화(synchronized)를 사용해 멀티쓰레드 환경에 안전하게 만드는 방법

```
public static synchronized Settings getInstance() { 
    if (instance == null) {
        instance = new Settings(); 
    }
    return instance; 
}
```

- static synchronized method
    - 인스턴스가 아닌 클래스 단위로 lock이 발생한다. 따라서 모든 스레드가 lock을 공유한다. 반면 인스턴스에 대한 lock은 서로 다른 인스턴스에 대한 lock을 공유하지 않는다.
    - 참고 : https://jgrammer.tistory.com/entry/Java-%ED%98%BC%EB%8F%99%EB%90%98%EB%8A%94-synchronized-%EB%8F%99%EA%B8%B0%ED%99%94-%EC%A0%95%EB%A6%AC

### 싱글톤 구현 패턴 3

> 이른 초기화 (eager initialization)을 사용하는 방법

```
private static final Settings INSTANCE = new Settings(); 
private Settings() {}

public static Settings getInstance() { 
    return INSTANCE;
}
```

- 이른 초기화의 단점은?
    - 사실 상태를 공유하지 않는 가벼운 객체들은 여러 개 생성해서 사용해도 크게 문제가 되지 않을 것이다.
    - 하지만 데이터베이스 연결 모듈이나 네트워크 통신 객체와 같이 무거운 객체들을 여러 번 생성한다면 ..?
    - 이런 것들은 한 번 생성해서 공유해서 사용하는게 좋지 않은가? 이런 생각으로부터(여러 이유가 있었겠지만) 싱글톤 패턴이 나왔다고 생각한다. 그래서 싱글톤 패턴은 주로 리소스를 많이 차지하는 역할을 하는 무거운 클래스에 적용할 때 적합하다고 한다.
    - 이 지점에서 이른 초기화의 단점을 생각해볼 수 있는데, 이른 초기화를 한다면 초반에 무거운 작업이 이루어진다. 그리고 이를 실제로 사용하지 않을 수도 있다. 이는 비효율적인 방식이다. 따라서 이른 초기화 대신 다른 방식을 고민해봐야 한다.

### 싱글톤 구현 패턴 4

> double checked locking으로 효율적인 동기화 블럭 만들기

```
public static Settings getInstance() { 
    if (instance == null) { //1번 if문 
        synchronized (Settings.class) { 
            if (instance == null) { //2번 if문
                instance = new Settings(); }
            } 
    }
    return instance; 
}
```

- double check locking이라고 부르는 이유?
    - 싱글톤 구현 패턴 2 방식은 스레드에 안전함에도 성능상 문제가 있을 수 있다. getInstance()를 호출할 때마다 잠재적으로 불필요한 잠금을 획득해야 한다. (이게 무거운 작업인가?)
    - 이 문제를 해결하기 위해 double check locking은 이중 검사를 통해 락을 획득하여 성능상 장점이 있다.
    - 스레드 A, 스레드 B, 스레드 C가 존재한다.
    - 스레드 A가 if분기문(1)을 평가하고 synchronized 내부로 진입한다.
    - 스레드 B도 if 분기문(1)을 평가하는데 이때 인스턴스가 생성되기 이전이므로 분기문을 통과한다.
    - 스레드 A가 lock을 얻고 synchronized 내부에서 작업중이므로 스레드 B는 기다린다.
    - 스레드 A가 인스턴스를 생성하고 블럭을 빠져나온다.
    - 스레드 B가 lock을 얻고 블럭 내부로 진입한다. 다시 if분기문(2)을 평가하는데 이때 instance는 null이 아니다. 따라서 블럭을 빠져나온다.
    - 스레드 C가 if분기문(1)을 평가한다. 인스턴스가 null이 아니므로 분기문을 빠져나온다.

- instacne 변수는 어떻게 정의해야 하는가? 그 이유는?
    - 멀티스레딩 환경에서 사용하는 변수는 기본적으로 캐시에 저장된다. 여러 스레드가 동일한 변수에 동시에 접근할 때 변수의 값을 캐시에 읽거나 쓰는 등의 문제가 생길 수 있다.
    - ![image](https://github.com/JINU-CHANG/dongguk-cafe/assets/98975580/1b03f915-e65f-4be8-a006-d1fd32346fea)
    - 스레드 A에서 instance 변수에 값을 할당했음에도 스레드 C가 다른 캐시에서 데이터를 읽는다면 instance 값이 null 일 수 있고 변수값 불일치 문제가 발생하게 된다.
    - 이런 문제를 해결하기 위해 CPU의 캐시를 거치지 않고 메인 메모리에 직접 read/write를 수행하는 volatile 변수를 사용한다.

## 싱글톤 패턴 구현을 깨뜨릴 수 있는 방법 1

### 리플렉션

> 구체적인 클래스 타입을 모르더라도, 그 클래스의 메서드, 타입, 변수들에 접근할 수 있도록 해주는 
자바 API

- 애플리케이션이 실행될 때 JVM의 메모리에는 바이트 코드로 변환된 소스코드의 클래스, 메서드, 필드 등의 정보들이 메타데이터를 포함하여 메모리의 static 영역에 적재되는데, 리플렉션은 런타임에 해당 메타데이터의 정보에 접근해 특정 클래스에 대한 정보를 획득한다.

```
Settings settings = Settings.getInstance();
Constructor<Settings> declaredConstructor = Settings.class.getDeclaredConstructor();
declaredConstructor.setAccessible(true);
Settings settings1 = declaredConstructor.newInstance();
System.out.println(settings == settings1);
```

- getDeclaredConstructors() : 해당 클래스에 선언된 모든 생성자를 배열의 형태로 반환한다. **여기에는 private으로 선언된 생성자도 포함된다.**
- getDeclaredConstructors()를 사용할 경우 **setAccessible(true)**라는 메서드를 추가로 작성해주는 것이 필요하다.
- newInstance()로 새로운 인스턴스를 만들면, 처음에 만든 settings와 다른 객체가 생성된다.

- 추가공부 필요
    - https://velog.io/@alsgus92/Java-Reflection은-무엇이고-언제어떻게-사용하는-것이-좋을까
    - https://thisisnew-storage.tistory.com/11#google_vignette

### 직렬화 역직렬화

> 직렬화 : Java의 객체를 외부에서도 사용할 수 있도록 바이트 형태로 데이터 변환하는 기술
역직렬화 : 바이트로 변환된 데이터를 다시 객체로 변환하는 기술

- https://steady-coding.tistory.com/576

```
Settings settings = Settings.getInstance();
Settings settings1 = null;

//직렬화 과정
try (ObjectOutput out = new ObjectOutputStream(new FileOutputStream("settings.obj"))) {
    out.writeObject(settings);
}

//역직렬화 과정
try (ObjectInput in = new ObjectInputStream(new FileInputStream("settings.obj"))) {
    settings1 = (Settings) in.readObject();
}
System.out.println(settings == settings1);
```

- **ObjectOutputStream**을 사용하여 파일에 **settings** 객체를 직렬화하고 저장한다.
- **ObjectInputStream**을 사용하여 직렬화된 객체를 역직렬화하고, **settings1** 변수에 할당한다.
- try-with-resource 블럭은 try문 탈출시 선언된 자원들을 모두 닫아주기 때문에 편리하다.

## 싱글톤 구현 패턴 6 → ENUM

> 리플렉션, 직렬화와 역직렬화에 안전한 싱글톤 구현 패턴

- enum으로 싱글톤 타입을 구현할 때의 단점은?
    - 싱글톤 구현 패턴 3에서와 같이 이른 초기화 (eager initialization)가 문제될 수 있다.
    - 대부분의 경우에는 크게 문제가 없겠지만, 리소스에 민감한 애플리케이션의 경우에는 신경써야 될 듯?
- 직렬화 & 역직렬화 시에 별도로 구현해야 하는 메소드가 있는가?
    - ENUM 자체가 Serializable을 구현하고 있기 때문에 별다른 메소드 추가없이도 직렬화&역직렬화가 된다.