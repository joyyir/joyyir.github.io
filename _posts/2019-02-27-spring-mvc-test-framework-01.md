---
title: "[Spring MVC] Test Framework : 개괄 및 Server-Side Tests"
description: Spring MVC Test Framework 3.6.1 Server-Side Tests 요약
excerpt : Spring MVC Server-Side Test 코드를 만들어보자!
date: 2019-02-27 00:00:00 -0400
categories:
  - Spring Framework
tags:
  - Spring
  - Spring MVC
  - Test
---

Spring Framework 공식 Documentation 요약 : [링크](https://docs.spring.io/spring/docs/current/spring-framework-reference/testing.html#spring-mvc-test-framework)

## Spring MVC Test Framework

* 실제 Servlet container를 실행하지는 않음 : `spring-test` 모듈의 Servlet API mock 객체를 사용하기 때문
* Spring MVC의 모든 런타임 행위를 제공하기 위해서 `DispatcherServlet`을 사용함
* TestContext framework를 통해서 실제 Spring configuration을 불러올 수 있음
* client-side testing 기능 제공 : `RestTemplate`을 사용함. 서버 응답을 mocking 하기 때문에 실제 서버를 띄우지는 않음 

### Server-Side Tests

* JUnit이나 TestNG의 한계점 : JUnit이나 TestNG로 Spring MVC Controller를 테스트할 경우에는 많은 부분이 테스트되지 않은 상태로 남아있음 : request mappings, data binding, type conversion, validation, 어노테이션이 붙은 controller 메소드 (@InitBinder, @ModelAttribute, @ExceptionHandler)
* Spring MVC Test는 이러한 한계를 뛰어 넘는다!
* Spring MVC Test의 목표는 실제 `DispatcherServlet`를 통해 controller를 테스트 하는 효율적인 방법을 제공하는 것임
* (예시) Spring MVC Test를 사용한 JUnit Jupiter-based 코드

```java
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.*;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;

@SpringJUnitWebConfig(locations = "test-servlet-context.xml")
class ExampleTests {

    private MockMvc mockMvc;

    @BeforeEach
    void setup(WebApplicationContext wac) {
        this.mockMvc = MockMvcBuilders.webAppContextSetup(wac).build();
    }

    @Test
    void getAccount() throws Exception {
        this.mockMvc.perform(get("/accounts/1")
                .accept(MediaType.parseMediaType("application/json;charset=UTF-8")))
            .andExpect(status().isOk())
            .andExpect(content().contentType("application/json"))
            .andExpect(jsonPath("$.name").value("Lee"));
    }
}
```
* 예시 코드 분석
    * TestContext framework의 `WebApplicationContext` 지원을 이용
        * 예시에서는 같은 패키지에 있는 XML configuration 파일을 통해 Spring configuration을 로딩했으나, Java-based, Groovy-based configuration도 로딩할 수 있다.
    * `/account/1`에 `GET` 요청을 보내고, 응답 상태는 `200`, 컨텐츠 타입은 `application/json`, 응답 바디의 JSON property `name`의 값은 `Lee`임을 테스트함
    * jsonPath syntax는 `Jayway JsonPath project`를 따름
* `MockMvc` 객체를 생성하는 두 가지 방법
    1. TestContext framework를 통해 Spring MVC configuration 로딩하기 (위의 예시)
        * 보다 완벽한 통합 테스트(integration test) 가능
        * TestContext framework에서 Spring configuration를 캐싱하기 때문에 여러 테스트를 돌려도 속도가 빠름
        * Spring configuration을 통해 mock service 객체를 주입할 수 있음. 그로 인해 web layer 테스트에 집중할 수 있음 
    2. `standaloneSetup` 사용 : Spring configuration을 로딩하지 않고 직접 controller 인스턴스를 만들기
        * 보다 단위 테스트에 가까움
        * mock 의존성을 가진 controller를 수동으로 주입할 수 있으며, `Spring configuration` 로딩을 하지 않음
        * 어떤 controller가 테스트되는지, 어느 특정 Spring MVC configuration이 요구되는지 등을 코드에서 쉽게 확인할 수 있음
        * 프로젝트의 Spring MVC configuration을 검증하는 추가적인 `webAppContextSetup` 테스트가 필요함을 암시
        * 예시 코드

```java
public class MyWebTests {

    private MockMvc mockMvc;

    @Before
    public void setup() {
        this.mockMvc = MockMvcBuilders.standaloneSetup(new AccountController()).build();
    }
    // ...
}
```

* MockMvc Setup
    * 예시 코드
```java
MockMvc mockMvc = MockMvcBuilders.standaloneSetup(new TestController())
    .defaultRequest(MockMvcRequestBuilders.get("/").accept(MediaType.APPLICATION_JSON)) // 요청 헤더에 `Accept` 헤더를 넣음
    .alwaysExpect(MockMvcResultMatchers.status().isOk()) // 항상 응답의 상태는 200을 예상함
    .alwaysExpect(MockMvcResultMatchers.content().contentType("application/json;charset=UTF-8")) // 항상 응답의 `Content-Type`은 json을 예상 
    .build();
// 여러 요청에 걸쳐서 Session을 공유할 수도 있음
MockMvc mockMvc = MockMvcBuilders.standaloneSetup(new TestController())
    .apply(SharedHttpSessionConfigurer.sharedHttpSession())
    .build();
```

* 요청을 수행 (Perform)
```java
mockMvc.perform(post("/hotels/{id}", 42).accept(MediaType.APPLICATION_JSON));
mockMvc.perform(multipart("/doc").file("a1", "ABC".getBytes("UTF-8")));
mockMvc.perform(get("/hotels?thing={thing}", "somewhere")); // URI template style, 디코딩 된다.
mockMvc.perform(get("/hotels").param("thing", "somewhere")); // 이미 디코딩 된 값이 들어가야한다.
```

* 요청 URI에서 contextPath와 servletPath를 따로 빼는 것이 선호되는 방법임
```java
public class MyWebTests {

    private MockMvc mockMvc;

    @Before
    public void setup() {
        mockMvc = standaloneSetup(new AccountController())
            .defaultRequest(get("/")
            .contextPath("/app").servletPath("/main")
            .accept(MediaType.APPLICATION_JSON)).build();
    }
```

* 기대치(Expectation) 정의하기
    * 기대치의 두 종류
        1. 응답의 속성 검증하기 : 응답 상태 코드, 헤더, 컨텐츠 등
        2. 응답을 넘어선 검증하기
            * 어떤 controller 메소드가 요청을 처리했는가?
            * 예외가 발생되고 처리되었는가?
            * model의 컨텐츠는 무엇인가? (???)
            * 어떤 view가 선택되었는가?
            * 어떤 flash attribute가 추가되었는가?
            * 등등
    * 예시
```java
mockMvc.perform(MockMvcRequestBuilders.get("/accounts/1")).andExpect(MockMvcResultMatchers.status().isOk());
``` 

* 수행된 요청을 dump 뜨고 싶을 때, `MockMvcResultHandlers`의 `print`메소드 활용하여 `System.out`으로 확인할 수 있다.
```java
mockMvc.perform(post("/persons"))
    .andDo(print())
    .andExpect(status().isOk())
    .andExpect(model().attributeHasErrors("person"));
```

* 결과에 대해 직접적인 접근을 하고 싶거나, 보다 깊게 검증하고 싶을 때 `MvcResult` 객체를 리턴받자.
```java
MvcResult mvcResult = mockMvc.perform(post("/persons")).andExpect(status().isOk()).andReturn();
// ...
```

* Servlet `Filter` 등록
    * 등록된 필터는 `spring-test`의 `MockFilterChain`을 통해 호출된다.
```java
mockMvc = standaloneSetup(new PersonController()).addFilters(new CharacterEncodingFilter()).build();
```

## Comment
* Spring MVC에서는 `DispatcherServlet`으로 테스트하기 때문에 Servlet Container에서와 유사한 환경에서 Controller 테스트가 가능할 것 같다.
* Builder 패턴으로 setup, perform, assert 하도록 코드를 작성할 수 있다. 아주 간결해서 가독성이 좋게 느껴진다. 