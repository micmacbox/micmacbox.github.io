---
layout: post
title: Event Loop (이벤트 루프)
categories: etc
tags: [frontend]
date: 2022-04-04 15:32 +0800
# toc: true
---

![](/assets/img/img_event_loof.png)
출처: [How JavaScript works: an overview of the engine, the runtime, and the call stack](https://extbrain.tistory.com/103)
자바스크립트는 단일스레드(= 하나의 stack을 가지고 있다) 프로그래밍 언어이기 때문에 한번에 여러가지의 작업을 수행 할 수 없다.
한번에 여러가지 작업을 수행할 수 없는데 어떻게 비동기로 작업이 되는걸까?
그 비밀은 Event Loop와 Callback Queue에 있다.

## JS Engine

위 이미지로 보다시피 자바스크립트 엔진은 memory Heap/Call Stack으로 구성되어있다.
Memory Heap : 메모리 할당이 일어나는 곳
Call Stack : 코드가 실행될 때 쌓이는 곳. stack 형태로 쌓임.

## Callback Queue

비동기적으로 실행된 콜백함수가 보관되는 영역.
click 이벤트 발생했을때 실행 될 콜백함수를 Callback Queue에 보관한다.

## Web API

Web Api는 브라우저에서 제공하는 API로 DOM이벤트, XMLHttpRequest이벤트, Timeout 작업등을 가능하도록 지원한다.

1. Call Stack에서 실행된 비동기 함수는 Web API를 호출
2. Web API는 콜백함수를 Callback Queue에 넣는다.
3. Call Stack이 비어진것을 확인하면 이벤트루프가 Callback Queue의 첫번째 콜백을 Call Stack으로 넣어준다.
4. 이러한 반복적인 동작을 틱(tick)이라고 한다.
