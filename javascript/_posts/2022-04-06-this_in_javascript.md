---
layout: post
title: this
categories: javascript
tags: [frontend, javascript]
date: 2022-04-06 15:32 +0800
# toc: true
---

javascript에서 this는 기본적으로 실행컨텍스트가 생성될때 결정된다.
실행컨텍스트는 함수가 호출될 때 생성되어 this는 함수가 호출될 떄 결정된다.

## 전역 공간에서의 this

전역 공간에서 this는 전역 객체를 가리킨다.
브라우저 환경에서 전역객체는 **window**이고 Node.js 환경에선 **global**이 된다.

자바스크립트의 모든 변수는 실행 컨텍스트의 LexicalEnvironment의 프로퍼티로서 동작한다. 이후 어떤 변수를 호출하면 L.E를 조회해서 일치하는 프로퍼티가 있을 경우 그 값을 반환한다. 전역 컨텍스트의 경우 L.E는 전역객체를 참조한다. (정확히는 전역객체 <- GlobalEnv <- 전역컨텍스트의 L.E 순으로 참조)

```
var a = 1;
console.log(a); 		//1
console.log(window.a) 	//1
console.log(this.a) 	//1
```

delete 명령에 대해선 전역변수 선언과 전역객체 프로퍼티 할당은 다르게 동작한다.

```
var a = 1;
delete window.a;					//false
console.log(a, window.a, this.a);	//1 1 1

window.b = 2;
delete window.b;					//true
console.log(b, window.b, this.b)	//uncaught ReferenceError: b is not defined
```

참고: 코어 자바스크립트
