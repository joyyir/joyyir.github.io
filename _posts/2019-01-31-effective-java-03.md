---
title: "[Effective Java 3/E] 아이템 3 - private 생성자나 열거 타입으로 싱글턴임을 보증하라"
description: Effective Java 아이템 3 싱글턴 패턴 요약
excerpt : 열거 타입 방식으로 싱글턴 객체를 만들자!
date: 2019-01-31 00:00:00 -0400
categories:
  - Effective Java
tags:
  - Java
  - Design Pattern
---
## 내용 요약

* 싱글턴 (singleton) : 인스턴스를 오직 하나만 생성할 수 있는 클래스
* 클래스를 싱글턴으로 만들면 이를 사용하는 클라이언트를 테스트하기가 어려워질 수 있다.
    * 싱글턴 인스턴스를 mock으로 대체할 수 없기 때문
    * 인터페이스를 정의해서 인터페이스를 구현해서 만든 싱글턴이라면 mock 가능
* 싱글턴 구현 방법
    * public static final 필드 방식
```java
public class Elvis {
    public static final Elvis INSTANCE = new Elvis();
    private Elvis() { ... }
}
```

        * 장점
            * 해당 클래스가 싱글턴임이 API에 명백히 드러남
            * public static 필드가 final이므로 절대로 다른 객체를 참조할 수 없음
            * 간결함
    * 정적 팩터리 방식
```java
public class Elvis {
    public static final Elvis INSTANCE = new Elvis();
    private Elvis() { ... }
    public static Elvis getInstance() { return INSTANCE; }
}
```
        * 장점
            * API를 바꾸지 않고도 싱글턴이 아니게 변경할 수 있음
            * 정적 팩터리를 제네릭 싱글턴 팩터리로 만들 수 있음 (이게 뭐야)
            * 정적 팩터리의 메소드 참조를 공급자(Supplier)로 사용할 수 있음
                * 가령, Elvis::getInstance를 Supplier<Elvis>로 사용하는 식
        * 위의 두 가지 방법은 직렬화 상황이나 리플렉션 공격에 취약함 (인스턴스 유일성이 보장되지 않음)
    * 열거 타입 방식
```java
public enum Elvis {
    INSTANCE;
    public void leaveTheBuilding() { ... }
}
```
        * 대부분 상황에서 가장 좋은 방법
        * 직렬화 상황이나 리플렉션 공격에서 자유로움
        * 단, Enum 외의 클래스를 상속해야 한다면 이 방법은 사용 불가 (인터페이스 구현은 가능)

## 느낀 점
* 싱글턴 객체를 사용하는 클라이언트를 테스트하기 어렵다는 점을 알게 되었다. 테스트 가능성을 위해서 인터페이스를 구현하는 방법으로 구현해야겠다.
* enum으로 싱글턴을 구현한 것을 한 번도 본 적 없는데, 이 책에서는 중요하게 다루고 있어서 흥미롭다. 현업에서는 직렬화 상황이나 리플렉션 공격은 없을거라고 가정했기 때문이 아닐까 싶다. 탄탄한 코드를 작성하려면 고려할 점이 정말 많구나라는 생각이 새삼 든다.