## call by value vs call by reference

함수의 매개변수에서 **값을 복사하느냐** **주소값을 참조하느냐**에 따라 나뉜다. 어떤 방식이냐에 따라 결과값이 달라지기 때문에 프로그래밍에서 중요하게 여겨진다.

**call by value**

```
int var = 10;

addValue(var);

public void addValue(int var_arg) {
    var_arg += 100;
}
```

addValue의 파라미터로 var이라는 변수 자체를 보냈지만 기본형(primitive) 변수의 값은 복사되어 전달되기 때문에 var_arg라는 새로운 변수에 값이 복사되어 저장된다.

즉 var과 var_arg는 같은 값을 지니지만 서로 다른 메모리 주소위에 적재되어 있다.

**자바는 call by reference가 아니다**

```
int[] var = {10,20,30};

int[] var_arg = addValue(var);

public int[] addValue(int[] var_arg) {
    var_arg[0] += 100;
    return var_arg;
}

System.out.println(var[0]); //1번
System.out.println(var_arg[0]); //2번
```

1번 결과값과 2번 결과값을 예측보라.

자바는 call by value이기 때문에 addValue의 파라미터로 var의 값이 복사된다. 이때 복사되는 값은 참조형(reference type) 변수일 경우 **변수의 주소값**이 된다. 따라서 var과 var_arg는 힙 영역의 동일한 객체를 가리키므로 1번과 2번이 출력하는 값을 동일하게 110이 된다.

또한 주소값을 비교하는 ``` System.out.println(var == var_arg);``` 다음의 값도 true로 출력된다.

-> call by reference는 그러면 어떻게 동작하는가 ?