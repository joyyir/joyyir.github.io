---
title: "[Spring MVC] Test Framework"
description: Spring MVC Test Framework 요약
excerpt : Spring MVC Test 코드를 만들어보자!
date: 2019-02-27 00:00:00 -0400
categories:
  - Spring
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

* Setup Features
    * 추후 정리

## Comment
* 추후 정리