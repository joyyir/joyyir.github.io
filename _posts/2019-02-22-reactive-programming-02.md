---
title: "[ReactiveX] Observable이란?"
description: ReactiveX Docs > Observable 요약
excerpt : Observable이란 뭘까?
date: 2019-02-22 00:00:00 -0400
categories:
  - Reactive Programming
tags:
  - Reactive Programming
  - ReactiveX
  - RxJava
---

ReactiveX 공식 홈페이지 Docs > Observable 요약 : [링크](http://reactivex.io/documentation/observable.html)

## Observable
> An observer subscribes to an Observable.

* Observable는 observer가 구독(subscribe)하는 대상임
* Observable가 아이템을 방출하면 observer가 반응(react)함
* 이 패턴은 동시성 연산(concurrent operation)을 편하게 해줌
    * 블록 상태에서 Observable이 아이템을 방출하길 기다릴 필요가 없음. 대신에, observer가 보초처럼 서서 적절하게 반응(react)할 준비를 하고 있음

## 배경
* ReactiveX에서는 Observer에 의해 명령어들이 병렬(paralell)로 실행되고, 결과값이 임의 순서로 나중에 반환된다.
* 데이터를 어떻게 가져오고 변환할지에 대한 매커니즘을 Observable 형태로 정의를 한다. 그 다음, 이 Observable을 observer가 구독함으로써 이 매커니즘이 동작을 시작 하고, Observable이 아이템을 방출할 때마다 observer는 정의된 매커니즘대로 동작한다.
* 이러한 방식의 장점은, 서로 의존성 없는 작업들을 처리해야 할 때, 작업들이 순서대로 하나 하나 끝나길 기다릴 필요 없이, 한 번에 여러 작업들을 동시에 시작할 수 있다는 점이다.

## Method call vs Asynchronous model
* Method call
```java
// make the call, assign its return value to `returnVal`
returnVal = someMethod(itsParameters);
// do something useful with returnVal
```
    1. 메소드를 호출한다.
    2. 변수에 메소드 반환 값을 저장한다.
    3. 변수 값으로 다른 유용한 일을 한다.
* Asynchronous model
```java
// defines, but does not invoke, the Subscriber's onNext handler
// (in this example, the observer is very simple and has only an onNext handler)
def myOnNext = { it -> do something useful with it };
// defines, but does not invoke, the Observable
def myObservable = someObservable(itsParameters);
// subscribes the Subscriber to the Observable, and invokes the Observable
myObservable.subscribe(myOnNext);
// go on about my business
```
    1. 비동기 호출에서 반환되는 값으로 할 일을 메소드로 정의한다. 이 메소드는 observer의 일부이다.
    2. 비동기 호출을 Observable 형태로 정의한다.
    3. Observable에 observer를 구독시킨다. (이는 Observable 동작을 시작시킴)
    4. 다른 할 일을 한다. (Observable이 아이템을 방출할 때마다 observer의 메소드가 아이템을 알아서 처리함)

## onNext, onComplete, onError
* observer라면 이 세 가지 메소드를 구현한다.
* Observable은 특정 상황에 observer의 메소드를 호출한다.
* onNext는 방출(emission)이고, onError, onComplete는 알림(notification)이다.
1. onNext : Observable이 아이템을 방출했을 때 호출함. 메소드 파라미터로 방출된 아이템을 전달함
2. onError : Observable이 예상된 데이터를 생성하는데에 실패했거나 다른 에러를 만났을 때 호출한다. 메소드 파라미터로 어떤 에러가 발생했는지를 전달함. 이후 onNext나 onComplete는 실행되지 않는다.
3. onComplete : Observable이 에러를 발생시키지 않고 onNext를 마지막으로 실행했을 때 호출함

## 구독 취소 (Unsubscribing)
* ReactiveX의 특화된 observer interface인 `Subscriber`에는 `unsubscribe` 메소드를 구현한다.
* Observable에 구독하는 observer가 없다면, Observable은 새로운 아이템 방출을 멈출지 선택할 수 있다.
* 구독 취소는 연산자(operator)에 전파(cascade back)되면서 아이템 방출을 막는다. 이는 동시에 발생된다고 보장되진 않는다. 또한 Observable에 구독하는 observer가 없더라도 잠깐동안은 아이템을 방출할 수도 있다.

## "Hot" and "Cold" Observables
* Observable은 언제 아이템 방출을 시작할까?
    * Hot : Observable이 생성되자마자 방출 시작
        * observer은 언제나 중간부터 아이템을 받을 수 있다.
    * Cold : Observable이 구독되었을 때 방출 시작
        * observer는 처음부터 아이템을 받을 수 있다.
    * `Connectable` Observable : observer 유무와 상관 없이 `Connect` 라는 메소드가 실행되기 전까지는 아이템을 방출하지 않는다.

## Observable 연산을 통한 조합
* Observables and observers은 표준 옵저버 패턴의 약간의 확장에 불과하다.
* ReactiveX의 강력함은 연산자(operator)에서 제공한다.
* 체이닝도 가능!
* 연산자에 대한 내용은 따로 정리 예정

## Comment
* Observable은 서로 의존성 없는 작업들을 동시에 처리할 때 유용하다는 점이 큰 장점으로 와 닿는다. API의 응답 시간을 길게 많드는 요인 중에 하나가 의존성 없는 API들을 순차적으로 실행하는 것이기 때문이다. 개발자들도 순차적으로 실행했을 때 성능이 떨어진 다는 것을 알지만, 병렬 처리를 구현하는 것이 복잡해서 선뜻 변경을 못하게 되는 것 같다. Observable을 사용해서 병렬 처리가 쉽게 구현된다면, 성능을 개선하는데 크게 도움이 될 것 같다.