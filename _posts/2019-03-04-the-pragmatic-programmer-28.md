---
title: "[실용주의 프로그래머] 28장 - 시간적 결합"
description: The pragmatic programmer 28장 요약
excerpt : 동시성을 고려하라!
date: 2019-03-04 00:00:00 -0400
categories:
  - The Pragmatic Programmer
tags:
  - Design
---

## Summary
* 시간의 두 가지 측면
  1. 동시성
  2. 순서
  
* 시간적 결합 (temporal coupling) : "메서드 A는 언제나 반드시 메서드 B보다 먼저 호출해야 한다."
  * 이러한 접근 방법은 유연하지 않고 현실적이지도 않음
  * 우리는 '동시성'을 허용할 필요가 있음
  * 그러므로써 1) 작업흐름 2) 아키텍처 3) 설계 4) 배치(deploy) 측면에서 시간과 관련된 의존성을 줄일 수 있음

1. 동시성 발견을 위한 작업 흐름 분석
  * UML activity diagram 을 활용하여 사용자들의 작업 흐름을 모델화하고 분석한다.
  * 아래 예시와 같이 도식화 되었다면, `Adding Coffee`와 `Steaming Milk`는 동시에 처리 가능함을 발견할 수 있다.

    <figure class="align-center" style="width: 300px">
        <img src="{{ site.url }}{{ site.baseurl }}/assets/images/UML-Activity-Diagram-23-1.png" alt="">
        <figcaption>https://www.geeksforgeeks.org/unified-modeling-language-uml-activity-diagrams/</figcaption>
    </figure>

2. 동시성을 극대화한 아키텍처
   * OLTP (On-Line Transaction Processing) 시스템의 예시 (hungry consumer model)
     * 모든 컴포넌트는 동시에 실행되는 독립적인 존재임
     * 컴포넌트의 집합이 n개의 계층으로 구성됨
     * 앞단 계층에서는 요청을 작업 큐(queue)에 보내고, 뒷단 계층에서는 큐에 쌓여있는 요청을 처리함 (consumer)
     * 앞단 계층에서는 요청을 작업 큐에 보내고나서 대기하는 것이 아니라, 바로 다음 요청을 받아들일 수 있는 상태가 됨 (**비동기!!!**)
     * 각 컴포넌트는 서로 시간적 결합이 끊기게 된다.

3. 동시성을 고려한 설계
   * 멀티 쓰레드 프로그래밍은 설계상의 제약이 존재하는데 이는 좋은 것이다. 이러한 제약은 멀티 쓰레드 프로그래밍이 아니더라도 코드의 결합을 끊어내는데 도움이 된다.
     1. 전역 변수나 정적 변수들의 동시 접근으로부터 보호해야 함
        * 애초에 전역변수가 필요 없다면 제거할 수 있는 기회이다.
     2. 객체는 호출될 때 언제나 유효한 상태에 있어야 함
        * 클래스 불변식을 사용하면 이 함정을 피할 수 있다. (추후 정리 예정)
      
4. 동시성을 고려하면 더 깔끔한 인터페이스로 설계가 가능함
  * C의 strtok 함수 예시 : thread safe 하지 않을 뿐만 아니라 동시에 두 문자열을 파싱하는 것도 불가능
  * 이에 반해, Java의 StringTokenizer는 thread safe하고 상태가 일관성 있음

5. 배치
  * 애플리케이션의 배치(예를 들어, 독립 애플리케이션, 클라이언트-서버, n-티어)를 결정할 때에도 유연하게 대응할 수 있음
  * **비동시적 애플리케이션에 동시성을 추가하려고 하는 것은 반대의 경우보다 훨씬 힘들다. 동시성을 허용하도록 설계한다면, 확장 가능성이나 성능에 대한 요구사항이 들어올 때 보다 쉽게 대응할 수 있고, 그런 일이 없더라도 여전히 깔끔한 설계의 이점을 누리게 됨**

## Comment
* 마지막 파트 (5. 배치) 에서 볼드 표시한 부분이 제일 중요하다고 생각되는 부분이다. 최근에 공부하는 Reactive Programming도 이러한 장점을 극대화하는데 큰 도움이 될 것 같다. 