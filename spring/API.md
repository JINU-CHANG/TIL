## 스프링 웹 개발 기초

스프링 웹 개발을 하는 3가지 방법이 있다.
* 정적 컨텐츠
* MVC와 템플릿 엔진
* API

지금까지 진행해온 프로젝트는 Spring MVC로 RESTful API 서비스를 구현했다고 볼 수 있다.

## API(Application Programming Interface)란 ?

**Application Programming** 은 응용 프로그램을 뜻하고, **Interface** 는 경계, 즉 서로 다른 두 개의 시스템이 정보를 주고 받도록 이어주는 경계를 뜻한다.

> 응용프로그램에서 사용할 수 있도록, 운영체제나 프로그래밍 언어가 제공하는 기능을 제어할 수 있게 만든 인터페이스를 뜻한다. 즉, 응용프로그램이 어떤 프로그램이 제공하는 기능을 사용할 수 있게 만든 **매개체**다. 

이전의 서버는 백엔드에서 데이터를 이용해서 완성된 HTML 파일을 브라우저에게 전달해주고, 브라우저는 단순히 뷰어 역할로 쓰였다.

요즘 서버는 브라우저에게 필요한 **데이터** 만을 전달하는 API 서버로 변화하고 있다. 데이터 전달 형식으로는 **JSON, XML**이 있는데 일반적으로 **JSON**이 많이 사용된다.

### REST API?

> 네트워크 아키텍처 스타일 (네트워크 자원을 정의하고 처리하는 방법 전반)

> REST는 HTTP를 잘 활용하기 위한 원칙이라고 할 수 있고, REST API는 이 원칙을 준수해 만든 API이다.

### HTTP를 잘 활용하기 위한 원칙 ?

이는 자원의 표현으로 '상태'를 전달하는 것이다.

URL로는 자원을 표현하는 데 집중하고, 자원의 상태(행위)에 대한 정의는 HTTP Method로 한다.

### REST API 예시

```
GET /members/delete/1
```

위의 방식은 REST를 제대로 적용하지 않은 URI이다. URI는 자원을 표현하는데 중점을 두어야 하는데, 'delete'와 같이 행위에 대한 표현이 들어갔기 때문이다. 따라서 아래와 같이 수정할 수 있다.

```
DELETE /members/1
```

## @ResponseBody 사용

@ResponseBody를 사용하면 HTTP의 BODY에 문자 내용이나 객체를 반환해준다.

객체를 반환하면 객체가 **JSON** 형식으로 변환된다.

### @ResponseBody 사용 원리

<img width = "500" src="https://github.com/JINU-CHANG/JINU-CHANG/assets/98975580/58f5323b-776d-496c-a27c-d53729a6c39f">

* **viewResolver** 대신에 **HttpMessageConverter** 가 동작한다.
* 기본 문자처리 : StringHttpMessageConverter
* 기본 객체처리 : MappingJackson2HttpMessageConverter


참고
* https://uhhyunjoo.tistory.com/50
* https://bentist.tistory.com/37