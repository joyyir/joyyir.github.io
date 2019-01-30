---
title: "[Effective Java 3/E] 아이템 2 - 생성자에 매개변수가 많다면 빌더를 고려하라"
description: Effective Java 아이템 2 빌더 패턴 요약
excerpt : 매개변수가 많다면 빌더를 사용하라!
date: 2019-01-30 00:00:00 -0400
categories:
  - Effective Java
tags:
  - Java
  - Design Pattern
---
## 내용 요약

* 점층적 생성자 패턴 (telescoping constructor pattern)
```java
public Person(int age) {
    this(age, false);
}
public Person(int age, boolean wearGlasses) {
    this.age = age;
    this.wearGlasses = wearGlasses;
}
```
    * 단점
        1. 사용자가 설정하길 원하지 않는 매개변수에도 값을 넣어줘야 함
        2. 매개변수 개수가 많아지면 코드를 작성하거나 읽기 어려움
* 그럼 매개변수가 많으면 어떻게 할까?
* 자바빈즈 패턴
```java
Person person = new Person();
person.setAge(10);
person.setWearGlasses(true);
```
    * (심각한) 단점
        1. 객체 하나를 만들려면 메서드를 여러 개 호출해야하고, 객체를 완성하기 전까지는 일관성(consistency)이 무너진 상태에 놓임
즉, 클래스를 불변으로 만들 수 없음
* 빌더 패턴 (Builder Pattern)
```java
public class Person {
    private final int age;
    private final boolean wearGlasses;

    public static class Builder {
        // 필수 매개변수
        private final int age;

        // 선택 매개변수
        private boolean wearGlasses = false;

        public Builder(int age) {
            this.age = age;
        }

        public Builder wearGlasses(boolean wearGlasses) {
            this.wearGlasses = wearGlasses;
            return this;
        }
        public Person build() {
            return new Person(this);
        }
    }

    private Person(Builder builder) {
        this.age = builder.age;
        this.wearGlasses = builder.wearGlasses;
    }
}
```
```java
Person person1 = new Person.builder(10).build();
Person person2 = new Person.builder(10).wearGlasses(true).build();
```
    * Setter 메서드들은 빌더 자신을 반환하기 때문에 연쇄적으로 호출 가능 => fluent API, method chaining 이라고 함
    * 장점
        1. 쓰기 쉽다
        2. 읽기 쉽다
        3. 필요한 매개변수만 세팅하고 나머지는 기본값으로 둘 수 있다
        4. 계층적으로 설계된 클래스와 함께 쓰기 좋다
            * 정리 생략 (책 본문 참조)
    * 잘못된 매개변수 일찍 발견하기 : 빌더의 생성자와 메서드에서 각 입력 매개변수의 invariant를 검사하고, build 메서드에서 여러 매개변수에 걸친 invariant를 검사하자. 잘못된 점이 발생하면 오류 메시지를 담아 IllegalArgumentException을 던지자
    * immutable vs invariant
        * immutable : 어떠한 변경도 허용하지 않는 것 (ex. String 객체)
        * invariant : 반드시 만족해야하는 조건 (ex. 배열의 크기는 0 이상이어야 함)
    * 단점
        1. 빌더 생성 비용이 있음 (성능에 민감한 상황에서만 문제)
        2. 점진적 생성자 패턴보다는 코드가 장황하다 (매개변수 4개 이상은 되어야 유의미함)
    * 매개변수가 많아질 것 같으면 애초에 빌더로 시작하자. API는 시간이 지날수록 매개변수가 많아지는 경향이 있다.

## 느낀 점
* 최근에 검색엔진 호출 시에 필요한 파라미터를 담는 클래스를 구현할 일이 있었는데, lombok 라이브러리의 @Builder annotation으로 구현했었다. method chaining으로 구현한다는 정도만 알고 있었는데, 구체적인 구현 방법과 장단점을 정리할 수 있어서 좋았다.
* legacy 코드를 살펴보면 메서드에 매개변수가 하나씩 늘어나면서 새로운 메서드가 덕지덕지 생긴 모습이 많이 보인다. 애초에 개발자가 매개변수가 많아질 것을 예측하고 매개변수 덩어리를 하나의 클래스로 묶고, 빌터 패턴으로 객체를 생성하여 메서드에 넘겼더라면 훨씬 깔끔하겠다는 생각이 든다.
* 특히, 점증적 생성자 패턴을 사용하면 선택적인 매개변수까지 필수로 넣어줘야해서 꼴보기 싫은 경우가 많은데, 빌더 패턴은 가독성을 확실히 향상시켜준다는 점만으로도 큰 가치가 있는 것 같다.
