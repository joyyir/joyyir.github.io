---
title: "[Spring Framework] Transaction Management"
description: 스프링의 트랜잭션 관리 기능 알아보기
excerpt : 스프링의 트랜잭션 관리 기능 알아보기
date: 2020-01-06 00:00:00 -0400
categories:
  - Spring Framework
tags:
  - Spring
---

## Spring Transaction Management

- Spring Reference : [링크](https://docs.spring.io/spring/docs/5.2.2.RELEASE/spring-framework-reference/data-access.html#transaction)

### `@Transactional` 어노테이션 사용하기

- 스프링에서 제공하는 `org.springframework.transaction.annotation.Transactional` 어노테이션. 자바 표준인 `javax.transaction.Transactional`을 사용해도 됨 (단, 스프링이 제공하는 기능을 다 쓰지는 못함)
- 클래스에 붙일 수도 있고, 메소드에 붙일 수도 있음 (클래스에 붙일 경우, 클래스의 모든 메소드에 어노테이션을 붙인 효과)
- `@Transactional`의 디폴트 설정
  - 전파(propagation) 옵션은 PROPAGATION_REQUIRED.
  - 격리 수준(isolation level)은 ISOLATION_DEFAULT.
  - 트랜잭션은 read-write 가능.
  - 타임아웃 시간은 내부 트랜잭션 시스템의 디폴트값 (없을 수 있음)
  - `RuntimeException`이 발생하면 롤백 진행 (checked exception은 롤백하지 않음)
- 트랜잭션 전파 (Transaction Propagation)
  - `PROPAGATION_REQUIRED` (default)
  ![](/assets/images/tx_prop_required.png)
    - 하나의 `물리적 트랜잭션`만 사용함
    - 각 메소드 별로 `논리적 트랜잭션 스코프`가 만들어짐
    - 각 `논리적 트랜잭션 스코프`는 rollback-only 상태를 개별적으로 관리함. 논리적으로 바깥 트랜잭션 스코프와 안쪽 트랜잭션 스코프는 독립적임
    - 이러한 스코프들은 결국 같은 물리적 트랜잭션에 매핑됨. 따라서 안쪽 트랜잭션 스코프에서 rollback-only로 마킹된 것이 바깥 트랜잭션에서 커밋할지 말지 여부에 영향을 줌
  - `PROPAGATION_REQUIRES_NEW`
  ![](/assets/images/tx_prop_requires_new.png)
  - `PROPAGATION_NESTED`