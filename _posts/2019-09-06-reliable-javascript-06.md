---
title: "[자바스크립트 패턴과 테스트] 6장 - 프라미스 패턴"
description: 자바스크립트 패턴과 테스트 6장 프라미스 패턴 요약
excerpt : 프라미스를 잘 써보자
date: 2019-09-06 00:00:00 -0400
categories:
  - Reliable JavaScript
tags:
  - JavaScript
---

## 6.1 단위 테스트
- Promise : 비동기 작업과 그 결과를 갖고 해야 할 일을 캡슐화한 객체로서, 작업 완료 시 이 객체에 캡슐화한 콜백을 호출한다.
- 콜백은 귀결(resolve) 콜백과 버림(reject) 콜백으로 나뉜다.

### 6.1.1 프라미스 사용법
- Promise 사용된 코드의 테스트코드 짜기 : [github commit](https://github.com/joyyir/006844/commit/7a7e5bcd605e8cd187da97def3f72c5befb46789)
    - Promise에 '이르렀을 때' 기대식을 평가하도록 유의
    - 재스민 사용하여 비동기 코드 테스트 시에는 done()을 쓸 것
- 상태와 숙명
    - Promise는 세 가지 상태(state)를 가짐 : 이룸(fulfilled), 버림(rejected), 보류(pending)
    - Promise는 두 가지 숙명(fate)를 가짐 : 귀결(resolved), 미결(unresovled)
    - 미결 프로미스는 항상 보류 상태이지만 귀결 프로미스는 세 가지 상태 중 하나가 될 수 있음
