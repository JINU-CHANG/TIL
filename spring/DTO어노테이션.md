## DTO에 @NoArgsConstructor(기본 생성자)가 필요한 이유

프로젝트중 DTO가 제대로 생성되지 않는 이슈가 있었다. 디버깅 끝에 DTO에 기본 생성자를 붙여주지 않아 생긴 문제임을 알았다. 왜 기본 생성자를 반드시 붙여줘야하는지 알아보자.

DTO에는 다른 생성자가 존재하더라도 @NoArgsConstructor(기본 생성자)를 통해 기본 생성자를 추가해줘야 한다.

```
@Getter
@NoArgsConstructor
public class PostCreateRequest {

    private String title;
    private String brand;
    private int purchasePrice;
    private String history;
    private Size size;
    private WearNum wearNum;

    private PostCreateRequest(String title, String brand, Size size, int purchasePrice, WearNum wearNum, String history) {
        this.title = title;
        this.brand = brand;
        this.size = size;
        this.purchasePrice = purchasePrice;
        this.wearNum = wearNum;
        this.history = history;
    }
}
```

이는 스프링이 DTO를 JSON으로 맵핑하는 원리와 연관되어 있다.

스프링은 Jackson 라이브러리의 **ObjectMapper**를 사용하여 JSON으로 매핑한다. Controller에서 DTO를 @RequestBody를 통해 가져올 때, 바인딩을 ObjectMapper가 수행한다. 이때 ObjectMapper는 DTO의 기본 생성자를 이용해 DTO를 생성하게 된다. 기본 생성자를 통해 DTO 객체를 생성하고 Setter 또는 Getter를 통해 DTO의 필드값들을 주입한다.

* 참고
    * https://ksh-coding.tistory.com/48