---
title: "[Java] ThreadPoolExecutor 알아보기"
description: Java의 ThreadPoolExecutor 알아보기
excerpt : ThreadPoolExecutor를 사용하여 비동기 작업을 동시에 처리해보자!
date: 2020-03-02 00:00:00 -0400
categories:
  - Java
tags:
  - Java
---

## ThreadPoolExecutor

- 출처 Java Documentation : [링크](https://docs.oracle.com/javase/7/docs/api/java/util/concurrent/ThreadPoolExecutor.html)

-----

- 제출된 task들을 스레드 풀의 여러 스레드를 이용해 실행하는 `ExecutorService` 구현체
- 예시 코드
```java
  @Test
  void test() {
      final int corePoolSize = 3;
      final int maximumPoolSize = 5;
      final int queueCapacity = 3;
      final ThreadPoolExecutor executor
          = new ThreadPoolExecutor(corePoolSize,
                                   maximumPoolSize,
                                   1L,
                                   TimeUnit.MINUTES,
                                   new ArrayBlockingQueue<>(queueCapacity));
      final Runnable task = () -> {
          System.out.println("thread:" + Thread.currentThread().getName());
          sleep(1000);
      };
      executor.submit(task);
      executor.submit(task);
      executor.submit(task);
      executor.submit(task);
      executor.submit(task);
      executor.submit(task);
      executor.submit(task);
      executor.submit(task);
      sleep(10000); // prevent main method termination
  }

  private void sleep(long millis) {
      try {
          Thread.sleep(millis);
      } catch (InterruptedException e) {
          e.printStackTrace();
      }
  }
```
- Console output
```text
thread:pool-1-thread-1
thread:pool-1-thread-2
thread:pool-1-thread-3
thread:pool-1-thread-4
thread:pool-1-thread-5
thread:pool-1-thread-5
thread:pool-1-thread-4
thread:pool-1-thread-1
```
- 팩토리 메소드로 간편하게 생성할 수도 있음
    - `Executors.newCachedThreadPool()` : 스레드 개수 제한이 없는 스레드풀
    - `Executors.newFixedThreadPool(int)` : 스레드 개수 제한이 있는 스레드풀
    - `Executors.newSingleThreadExecutor()` : 스레드가 1개만 있는 스레드풀
- 팩토리 메소드를 안 쓰고 직접 값을 설정해서 만들 수도 있음
- 수동으로 설정하고 튜닝할 수도 있음
    1. Core, maximum pool sizes
        - ThreadPoolExecutor를 생성한다고 바로 스레드가 생성되는 것은 아니다.
            - 현재 스레드 개수가 `corePoolSize` 개수 이하인 경우, 새로운 task를 받았을 때 무조건 새로운 스레드가 생성된다. (이미 생성된 스레드가 놀고 있어도 무조건 생성함)
            - 현재 스레드 개수가 `corePoolSize` 이상, `maximumPoolSize` 미만일 경우, queue가 꽉 찼을 때만 새 스레드가 생성된다.
        - `corePoolSize`와 `maximumPoolSize`를 동일하게 설정할 경우 fixed-size thread pool이다.
        - `maximumPoolSize`를 `Integer.MAX_VALUE`로 설정하면 임의의 수의 동시 작업을 수용하도록 허용하게 된다.
        - `corePoolSize`와 `maximumPoolSize`는 보통 생성자에서 설정하지만, setter를 통해 동적으로 변경할 수도 있다.
    2. Keep-alive times
        - corePoolSize보다 많은 스레드가 존재할 경우, 초과된 스레드가 `keepAliveTime` 시간동안 놀고 있으면 (idle) 자동으로 개수를 줄인다.
        - 역시 이 값도 setter를 통해 동적으로 변경할 수도 있다.
    3. Queuing
        - `BlockingQueue`가 사용된다.
            - `BlockingQueue`란? : 값을 가져오는 입장에서는 큐에 값이 들어오길 기다리고, 값을 저장하는 입장에서는 큐가 비워지길 기다리는 기능을 지원하는 큐. 구현체로는 `SynchronousQueue`, `LinkedBlockingQueue`, `ArrayBlockingQueue` 등이 있음
        - 이미 maximumPoolSize개 만큼 스레드가 생성된 상태에서 queue가 꽉 찬 경우, 해당 task는 거부된다. (`RejectedExecutionException` 발생)
        - Queuing 전략
            1. 바로 손 털어버리기 (Direct handoffs)
                - `SynchronousQueue`를 이용하여 task를 큐에 들고있지 않고 바로 스레드로 넘겨버리는 것. 이 전략은 보통 `maximumPoolSizes` 제한을 두지 않는데, 이는 처리량보다 요청량이 많을 경우 스레드 개수가 무한정 늘어날 수 있다.
                    - `SynchronousQueue`란? : `BlockingQueue`의 일종. 값을 내부 저장하지 않는다. peek 연산이 불가능하다. (remove 연산을 할 때만 값을 얻을 수 있기 때문) 서로 다른 스레드의 두 객체 간의 동기화가 이루어지는 핸드오프 디자인에 적합하다.
                - This policy avoids lockups when handling sets of requests that might have internal dependencies. ==> 큐잉 할 필요 없이 바로 처리되기 때문 아닐까?
            2. Unbounded queues 
                - 크기 제한이 없는 큐 (예시: `new LinkedBlockingQueue<>()`) 사용
                - 스레드는 최대 `corePoolSize`개 만큼 생성된다. (큐가 꽉 차야 새로운 스레드가 생길텐데, 큐 크기에 제한이 없으니… `maximumPoolSize`는 아무런 영향이 없는 값이 됨)
            3. Bounded queues
                - 크기 제한이 있는 큐 (예시: `new ArrayBlockingQueue(10)`) 사용
                - 유한한 `maximumPoolSizes`와 함께 사용되었을 때 불필요한 리소스 낭비를 막아줄 수 있다.
                - 하지만 적절한 튜닝과 제어가 어렵다. 큐 크기와 pool 크기는 trade-off 관계이므로 적절한 조절이 필요하다.
                    - 큐 크기는 큰데 pool 크기는 작을 경우 : CPU 사용, OS 자원, 컨텍스트 스위칭 비용은 줄일 수 있지만 처리량(throughput)이 낮아질 수 있다.
                    - 큐 크기는 작은데 pool 크기는 클 경우 : CPU를 더 바쁘게 쓸 수 있지만 스케줄링 오버헤드가 생겨서 오히려 처리량이 더 낮아질 수 있다.