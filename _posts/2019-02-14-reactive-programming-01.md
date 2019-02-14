---
title: "[ReactiveX] 소개 (Introduction)"
description: ReactiveX Introduction 요약
excerpt : ReactiveX란 뭘까?
date: 2019-02-14 00:00:00 -0400
categories:
  - Reactive Programming
tags:
  - Reactive Programming
  - ReactiveX
  - RxJava
---

ReactiveX 공식 홈페이지 Introduction 요약 : [링크](http://reactivex.io/intro.html)

## ReactiveX
* observable 시퀀스를 사용하여 비동기(asynchronous), 이벤트 기반(event-based) 프로그램을 구성하기 위한 라이브러리
* the observer pattern의 확장하여 데이터/이벤트 시퀀스를 지원하고, 연산자(operator)를 지원하여 시퀀스를 선언적으로 함께 구성할 수 있게 함
* 복잡한 문제들은 추상화시킴 (low-level threading, synchronization, thread-safety, concurrent data structures, and non-blocking I/O)
* asynchronous data도 synchronous data 처럼 다룰 수 있게 함
    
    |              | single items        | multiple items          |
    |--------------|---------------------|-------------------------|
    | synchronous  | T getData()         | Iterable<T> getData()   |
    | asynchronous | Future<T> getData() | Observable<T> getData() |


## 왜 Observable을 사용하는가?
* ReactiveX의 Observable 모델은 비동기 이벤트 스트림에 대해 각종 편리하고 강력한 연산자(operation)를 지원함
* 이 연산자들은 콜백 지옥에서 구출해 줄 뿐만 아니라, 가독성을 좋게 해주고 버그 가능성을 낮춰줌

1. Observable은 구성 가능함 (Composable)
    * Java Future의 경우 single level의 비동기 실행에 적합하다. 비동기 실행이 중첩될 경우 복잡도가 크게 증가한다.
    * 이에 반해 ReactiveX의 Observable은 비동기 데이터의 흐름(flow)과 연속(sequence)을 구성할 수 있도록 설계되었다.
    * flow vs sequence
        * flow : stream, emission이 진행 중
        * sequence : emission이 완료됨

2. Observable은 유연함 (Flexible)
    * Java Future는 하나의 스칼라 값의 생성만 지원하지만, Observable은 연속된 값 뿐만 아니라 무한한 스트림까지 지원한다.
    * Observable은 synchronous 방식인 Iterable이 가진 유연함과 우아함을 모두 가지고 있다.
    * Iterable과 Observable 비교

        | event          | Iterable (pull, sync) | Observable (push, async) |
        |----------------|-----------------------|--------------------------|
        | retrieve data  | T next()              | onNext(T)                |
        | discover error | throws Exception      | onError(Exception)       |
        | complete       | !hasNext()            | onCompleted()            |

3. Observable은 강요하지 않음 (Less Opinionated)
    * Observable은 (thread-pools, event loops, non-blocking I/O, actors)을 사용하여 구현될 수 있다.
    * Observable 구현이 어떻든 간에(블록킹이든, 논블록킹이든, 쓰레드풀이든, 이벤트루프이든) 클라이언트 코드는 Observable 관련 코드를 비동기로써 취급한다.
    * Observable 구현은 언제든 마음만 먹으면 바꿀 수 있고, 그럴 때마다 클라이언트 코드를 변경할 필요가 없다.
4. 콜백은 그들 자신의 문제가 있다 (무슨 말이죠???)
    * 콜백은 Future.get()에서의 너무 이른 블로킹이 되는 문제를 블록을 허용하지 않음으로써 해결한다. (이게 무슨 말???)
5. ReactiveX는 다양한 언어로 구현됨

## Reactive Programming
* ReactiveX는 다양한 연산을 지원함 (filter, select, transform, combine 등)
* Observable은 Iterable과 동일한 관점에서 프로그래밍 할 수 있다. **Iterable보다 Observable이 보다 유연한데, Iterable은 동기적 데이터만 처리할 수 있는 반면, Observable은 데이터가 동기적이든 비동기적이든 똑같은 코드로 처리할 수 있기 때문**이다.
![](/assets/images/iterableVsObservable.png)
* Iterable과 Observable이 일치하기 위해, the Gang of Four’s Observer pattern에는 없는 두 개의 기능이 추가됨
    * 생산자가 소비자에게 더 이상 데이터가 없음을 알릴 수 있음 (onComplete)
    * 생산자가 소비자에게 오류가 발생했다는 신호를 보낼 수 있음 (onError)
* 이 두 가지 추가 덕분에, Iterable에서 가능한 모든 연산을 Observable에서도 할 수 있게 됨

## Comment
* synchronous programming 사고방식에 너무 익숙해져서 비동기적으로 생각하는게 낯설기만하다. ReactiveX 학습은 이런 동기적 사고방식을 비동기적 사고방식으로 바꾸는 첫 관문이 될 것 같다.
* 이 문서에서는 Iterable보다 Observable이 보다 유연하다고 설명하고 있다. Observable을 사용하여 동기, 비동기에 대한 처리를 모두 할 수 있다면, 굳이 유연성이 떨어지는 Iterable 방식을 고집할 필요가 없을 것이다.