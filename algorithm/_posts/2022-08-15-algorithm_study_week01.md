---
layout: post
title: "[알고리즘 스터디] 1주차"
categories: algorithm
tags: [javascript, algorithm]
date: 2022-08-15 01:00 +0800
# toc: true
---

## ❔ 최대공약수와 최소공배수

[프로그래머스 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/12940)

_공약수(common divisor)란 두 수 이상의 여러 수의 공통된 약수를 의미한다.
최대공약수 GCD(Greatest Common Divisor): 공약수 중 최대인 수
최소공배수 LCM(Least Common Multiple): 두 수에 공통으로 존재하는 배수 중 가장 작은 수_

최대공약수는 유클리드 알고리즘을 이용하면 쉽게 풀 수 있다.

유클리드 호제법: a, b라는 수가 있다(a>b). a를 b로 나눈 나머지를 r이라고 했을 때 a,b의 최대공약수와 b,r의 최대공약수는 같다.
b를 r로 나눈 나머지는 r'이라고 했을때 b,r의 최대공약수는 r,r' 과 같다.
나머지가 0이 될 떄 까지 위의 과정을 반복 할 경우 a, b의 최대공약수를 얻을 수 있다. (O(logN))

## 문제풀이

```
function solution(n, m) {
  return [gcd(n, m), lcm(n, m)];
}

function gcd(n, m) {
  return n % m === 0 ? m : gcd(m, n % m);
}
function lcm(n, m) {
  return (n * m) / gcd(n, m);
}

```

## ❔ 문자열 내 마음대로 정렬하기

[프로그래머스 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/12915)

분명 예전에 풀었던 문제였는데 내가 짠 코드도 기억이 안났다 왜 이렇게 짰더라?^^ㅋㅋ...
String.prototype.charCodeAt() 으로 푸는 방법도 있고, 나는 n번째 문자 그대로를 비교하여 Boolean -> Num으로 리턴하여 풀었다.

## 문제풀이

```
function solution(strings, n) {
  var answer = [];

  answer = strings.sort((first, second) => {
    let a = first.charAt(n);
    let b = second.charAt(n);
    if (a === b) {
      return (first > second) - (first < second);
    } else {
      return (a > b) - (a < b);
    }
  });
  return answer;
}

```

## ❔ 하샤드 수

[프로그래머스 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/12947)

reduce를 이용하면 쉽게 풀 수 있는 문제.
주어진 숫자를 문자열로 바꿀 수 없는 조건이 걸린 경우엔 각 자릿수를 10으로 나눈 나머지 값을 더하여 자릿수의 합을 구해 풀 수도 있겠다.

## 문제풀이

```
function solution(x) {
  var num = [...x.toString()].reduce((acc, _) => {
    return _ * 1 + acc;
  }, 0);
  return x % num == 0 ? true : false;
}

```

## ❔ K번째수

[프로그래머스 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/42748)

내장함수들로 쉽게 접근이 가능했다.

## 문제풀이

```
function solution(array, commands) {
    var answer = commands.map(command => (array.slice(command[0]-1, command[1]).sort((a,b)=> a-b)[command[2]-1]));
    return answer;
}
```

## ❔ 완주하지 못한 선수

[프로그래머스 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/42576)

## 문제풀이

```
function solution(participant, completion) {
  participant.sort();
  completion.sort();

  for (let idx in participant) {
    if (participant[idx] != completion[idx]) return participant[idx];
  }
}
```

## ❔ 폰켓몬

[프로그래머스 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/1845)

배열 nums에서 중복값을 제거한 사이즈를 s라고 할 때
최대 값은 nums의 2분의 1이기 때문에 nums의 반값이 s보다 같거나 클 경우 s를 리턴 해주었다.

## 문제풀이

```
function solution(nums) {
    var s = new Set(nums).size;
    return (nums.length/2 >= s)? s : nums.length/2 ;
}
```
