---
title: "[Effective Java 3/E] 아이템 4 - 인스턴스화를 막으려거든 private 생성자를 사용하라"
description: Effective Java 아이템 4 private 생성자 요약
excerpt : 유틸리티 클래스의 인스턴스화를 막자!
date: 2019-02-07 00:00:00 -0400
categories:
  - Effective Java
tags:
  - Java
---

## Summary

* 단순히 정적 메서드와 정적 필드만을 담을 클래스를 만들고 싶을 때가 있다. (ex. 유틸리티 클래스) 이런 클래스는 인스턴스화 하려고 설계한 게 아니다. 생성자를 명시하지 않으면 컴파일러가 자동으로 생성자를 만들기 때문에 인스턴스화 될 위험성을 갖고 있다. 이럴 때, private 생성자를 추가하면 인스턴스화를 막을 수 있다.
```java
public class UtilityClass {
    // 기본 생성자가 만들어지는 것을 막는다.
    private UtilityClass () {
        throw new AssertionError(); // 클래스 내부에서 생성자를 호출하지 않도록 해준다.
    }
    // … 나머지 코드 생략
}
```
    * 이 방식은 상속을 불가능하게 하기도 함 (모든 생성자는 명시적/묵시적으로 상위 클래스의 생성자를 호출하기 때문)

## Comment
* 저런 유틸리티 클래스를 만드는 사람 입장에서는 ‘설마 다른 개발자가 이걸 인스턴스화 하겠어?’ 라고 생각하고 private 생성자로 처리하지 않는 경우가 많은 것 같다. 하지만 일관되고 자원이 낭비되지 않는 프로그램을 위해서 이 가이드를 따르도록 해야겠다.