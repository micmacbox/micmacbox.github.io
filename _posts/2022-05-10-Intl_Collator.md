---
layout: post
title: Intl.Collator
categories: etc
tags: [frontend]
date: 2022-05-10 15:32 +0800
# toc: true
---

업무 내용 중 다국어를 각 국가의 언어의 오름차순으로 정렬해야 하는 일이 있었다.
어떻게 처리해야할지 고민하던 차에 동료분의 도움으로 해당 기능을 이용하여 해결 할 수 있었다.

## Intl

Intl개체는 언어 구분 문자열 비교, 숫자 형식 지정, 날짜 및 시간 형식 지정을 제공하는 ECMAScript 국제화 API의 네임스페이스 객체이다.

## Intl.Collator

각 언어에 대한 문자열 비교를 제공하는 생성자

```
['Z', 'a', 'z', 'ä'].sort(new Intl.Collator('de', { caseFirst: 'upper' } ).compare);
// expected output: ["a", "ä", "Z", "z"]
```

참고: [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/Collator)
