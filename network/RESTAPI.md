### API(Application Programing Interface)란?

> API는 응용 프로그램에서 사용할 수 있도록, 운영 체제나 프로그래밍 언어가 제공하는 기능을 제어할 수 있게 만든 인터페이스를 뜻한다.

인터페이스는 기계끼리 정보를 교환하기 위한 수단을 의미하여, API는 쉽게 말하면 응용프로그램에서 데이터를 주고 받기 위한 방법이다.

예를 들어 내가 개발하고 있는 서비스에서 `날씨 데이터`가 필요한 경우 기상청의 날씨 API를 이용하면 날씨 데이터를 제공받을 수 있다.

### REST API란? 그리고 REST API가 잘 지켜지지 않는 이유

> REST API : 클라이언트와 서버 사이의 데이터를 주고받을 때 사용하는 아키텍처 스타일(규칙들의 집합)

`How do I improve HTTP without breaking the web`

개발자들은 위의 문제를 해결하기 위해 웹을 독립적으로 진화 가능하게 했다. 서버와 클라이언트는 서로 독립적으로 발전할 수 있다. 예를 들어 서버의 기능이 변경되었어도 클라이언트는 업데이트를 할 필요가 없어진다. 우리가 웹을 사용하면서 `6.0 버전으로 업데이트를 하라`라는 말을 본 적이 없는 것이 그 예이다. REST는 이렇게 웹의 독립적 진화를 위해 탄생했으며 여러 제약조건들을 통해 정의된다.

- client-server
- stateless
- cache
- uniform interface
- layered system
- code-on-demand

REST의 창시자에 의하면 이 규칙들을 모두 지킨 API를 REST API라고 칭한다. 대부분의 규칙은 HTTP를 사용하며 지켜진다. 하지만 REST API에서 **uniform interface**는 잘 지켜지지 않는다.

**Unifrom Interface의 제약조건**

- identification of resources
    - 리소스가 URI로 식별되어야 한다.
- manipuliation of resources through representations
    - 표현(representation)을 통해 자원을 조작할 수 있어야 한다.
- **self-descriptive messages**
    - 메시지는 스스로 설명해야 한다.
- **hypermedia as the engine of application state(HATEOAS)**
    - 애플리케이션의 상태는 Hyperlink를 통해 전이되어야 한다.

REST API는 JSON을 통해 데이터를 전달한다. 하지만 JSON 데이터는 key-value 값이 무엇을 의미하는지 스스로 설명하지 않는다. 데이터를 이해하기 위해 우리는 API 문서를 살펴봐야 한다. 또한 하이퍼링크를 따로 추가하지도 않는다. 따라서 우리가 사용하는 API는 엄격하게는 REST API가 아니다. HTTP API라고 부르는 것이 맞지만 이미 많은 사람들이 해당 조건을 지키지 않아도 REST API라고 하기 때문에 HTTP API나 REST API를 거의 같은 의미로 사용하고 있다.

### REST API 디자인 가이드

완벽하게 REST API를 설계할수는 없지만 통용되어 사용되는 REST API 디자인 가이드를 따르는 것이 중요하다. 원칙에 대한 내용은 아래 링크를 참고하자.

- https://meetup.nhncloud.com/posts/92

<br>

- 참고
    - https://www.youtube.com/watch?v=RP_f5dMoHFc
    - https://www.inflearn.com/questions/126743/http-api-vs-rest-api?gad_source=1&gclid=Cj0KCQiAwvKtBhDrARIsAJj-kTiKBjToBvXJdr-P1XUJ_JYZTsRgLxpsw_FFiGRAHY8Vtp5upg98KtgaAmjmEALw_wcB
    - https://meetup.nhncloud.com/posts/92

