---
layout: post
title: bind, call, apply 차이점
categories: javascript
tags: [javascript, es6]
date: 2022-02-07 15:32 +0800
# toc: true
---

## binding이란?

javascript 언어의 특징인 this의 정의를 알고있다면 이해가 쉬울것이다. this는 함수를 호출한 방법에 의해 결정된다.

1. 전역에서 호출하는 경우는 window 객체를 가리킨다.
2. 함수를 호출한 경우 함수 내부의 this의 값은 함수를 호출한 방법에 의해 결정된다.

## call, apply 메서드

첫번째 인자에 this를 가르킬 객체를 정해주는것은 같다.
두 메서드의 차이점은 실행 함수의 인자를 설정하는 방법이다.
**call**: 실행함수에 필요한 인자를 그대로 나열한다.
**apply**: 실행함수에 필요한 인자를 배열 형식으로 넣는다.

```
function add(c, d) {
  return this.a + this.b + c + d;
}

var o = {a: 1, b: 3};

// 첫 번째 인자는 'this'로 사용할 객체이고,
// 이어지는 인자들은 함수 호출에서 인수로 전달된다.
add.call(o, 5, 7); // 16

// 첫 번째 인자는 'this'로 사용할 객체이고,
// 두 번째 인자는 함수 호출에서 인수로 사용될 멤버들이 위치한 배열이다.
add.apply(o, [10, 20]); // 34
```

## bind 메서드

bind메서드의 첫번째 인자인 객체를 this로 가진 새로운 실행함수를 생성하며, 이 함수의 this는 영구적으로 첫번째 인자로 고정된다.

```
function f() {
  return this.a;
}

var g = f.bind({a: 'azerty'});
console.log(g()); // azerty

var h = g.bind({a: 'yoo'}); // bind는 한 번만 동작함!
console.log(h()); // azerty

var o = {a: 37, f: f, g: g, h: h};
console.log(o.a, o.f(), o.g(), o.h()); // 37, 37, azerty, azerty
```

## References

[MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/this)
