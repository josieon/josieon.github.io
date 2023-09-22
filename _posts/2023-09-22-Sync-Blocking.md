---
layout: post
title: Sync and Async, Blocking and Non-Blocking
---

간단하게 말하면, 관점의 차이이다.

Blocking / Non-Blocking은 호출된 함수가 호출한 함수에게 제어권을 바로 주느냐 안주느냐,  
Sync / Async는 호출된 함수의 종료를 호출한 함수가 처리하느냐, 호출된 함수가 처리하느냐의 차이다.

# Blocking

- A 함수가 B 함수를 호출할 때, B 함수가 자신의 작업이 종료되기 전까지 A 함수에게 제어권을 돌려주지 않는 것이다

# Non-Blocking

- A 함수가 B 함수를 호출할 때, B 함수가 제어권을 바로 A 함수에게 넘겨주면서, A 함수가 다른 일을 할 수 있도록 하는 것이다

### Blocking과 Non-Blocking은 요청에 대해 받은 쪽(호출된 함수)에서 처리가 끝나기 전에 리턴해주는가 아닌가의 여부(현재 작업이 Block되는지의 여부)

# Synchronous

- A 함수가 B 함수를 호출할 때, B 함수의 결과를 A 함수가 처리하는 것이다

# Asynchronous

- A 함수가 B 함수를 호출할 때, B 함수의 결과를 B 함수가 처리하는 것이다(callback)

### Sync와 Async는 처리 요청한 순서가 지켜지는가 아닌가(작업을 순차적으로 수행할지의 여부)

https://inpa.tistory.com/entry/%F0%9F%91%A9%E2%80%8D%F0%9F%92%BB-%EB%8F%99%EA%B8%B0%EB%B9%84%EB%8F%99%EA%B8%B0-%EB%B8%94%EB%A1%9C%ED%82%B9%EB%85%BC%EB%B8%94%EB%A1%9C%ED%82%B9-%EA%B0%9C%EB%85%90-%EC%A0%95%EB%A6%AC