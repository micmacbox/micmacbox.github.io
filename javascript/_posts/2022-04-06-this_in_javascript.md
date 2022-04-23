---
layout: post
title: 자바스크립트 this
categories: javascript
tags: [frontend, javascript]
date: 2022-04-23 15:32 +0800
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
사용자가 의도치않게 삭제하는 것을 방지하는 차원에서 처음부터 전역객체의 프로퍼티로 할당한 경우에만 삭제가 된다. 이는 전역변수를 선언하면 자바스크립트 엔진이 자동으로 전역객체의 프로퍼티로 할당하며 해당 프로퍼티의 configurable속성을 false로 정의하기 때문이다.

```
var a = 1;
delete window.a;					//false
console.log(a, window.a, this.a);	//1 1 1

window.b = 2;
delete window.b;					//true
console.log(b, window.b, this.b)	//uncaught ReferenceError: b is not defined
```

## 메서드로서 호출할때

### 함수vs메서드

함수와 메서드를 구분하는 차이는 *독립성*에 있다.
함수: 그 자체로 독립적 기능을 수행
메서드: 자신을 호출한 대상 객체에 관한 동작 수행

객체의 메서드로서 호출할 경우에만 메서드로 동작, 그렇지 않으면 함수로 동작한다고 생각하면 된다.

'함수로서 호출'과 '메서드로서 호출'을 어떻게 구분하는가?
:함수 앞에 객체가 명시 되어있는 경우는 메서드로서 호출한 것이다. (점 혹은 대괄호 표기법으로 호출한 경우)

```
    var func = function(x) {
	    console.log(this, x);
    };
    func(1); //window {...} 함수로서 호출

	var obj = {
		method: func
	};
	obj.method(2); //{method: f} 메서드로서 호출
```

### 메서드 내부에서의 this

this에는 호출한 주체에 대한 정보가 담기는데 어떤 함수를 메서드로서 호출하는 경우 호출주체는 함수명 앞의 객체이다.

```
	var obj = {
		methodA: function() {console.log(this);},
		inner: {
			methodB: function() {console.log(this);}
		}
	};
	obj.methodA(); //{methodA:f, inner:{...}} (===obj)
	obj.inner.methodB(); //{methodB:f} (===obj.inner)
```

참고: 코어 자바스크립트
