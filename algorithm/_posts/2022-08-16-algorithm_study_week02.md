---
layout: post
title: "[알고리즘 스터디] 2주차"
categories: algorithm
tags: [javascript, algorithm]
date: 2022-08-17 01:00 +0800
# toc: true
---

# ❔ 같은 숫자는 싫어

[문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/12906)

처음엔 중복을 완전 없애는 문제인줄 알고 Set을 이용했지만 테스트케이스 1번에서 걸러졌다.
filter를 사용해 이전 원소와 현재 원소가 같은지 체크하여 다른경우 해당원소를 배열에 추가 함으로써 해결.

## 문제풀이

```
function solution(arr) {
  return arr.filter((x, i) => x != arr[i - 1]);
}
```

<br>

# ❔ 크레인 인형뽑기

[문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/64061)

이거 레벨1 문제 맞나여? 아닌것 같은디?
board 배열의 행과 열을 바꿔준 다음 filter함수를 이용해 0을 제거한 스택을 만들어 하나씩 pop해줬다.

## 문제풀이

```
function solution(board, moves) {
  var answer = 0;
  let bascket = [];
  let stack = board.reduce((acc, cur, i, m) => {
    let trans = Array(m.length)
      .fill(0)
      .map((_x, _i) => m[m.length - 1 - _i][i])
      .filter(x => x != 0);
    acc.push(trans);
    return acc;
  }, []);

  for (let move of moves) {
    let poped = stack[move - 1].pop();
    if (!poped) continue;
    if (bascket.slice(-1)[0] === poped) {
      answer += 2;
      bascket.pop();
    } else bascket.push(poped);
  }
  return answer;
}

```

<br>

# ❔ 올바른 괄호

[문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/12909)

푸는 방법은 다양하지만 스택/큐로 어떻게 접근해야하나 고민했다

## 문제풀이

```
function solution(s) {
  let stack = [];
  for (_s of s) {
    if (_s === '(') {
      stack.push(_s);
      continue;
    }
    if (!stack.pop()) return false;
  }
  return !stack.length ? true : false;
}
```

<br>

# ❔ 기능개발

[문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/42586)

레벨2 치고는 쉽게 풀었다
그런데 워낙 사고하는 스피드가 느려서 많이 푸는 연습을 해야할듯

## 문제풀이

```
function solution(progresses, speeds) {
    var answer = [];
    let lefted = progresses.map((x, i)=> Math.ceil([100-x]/speeds[i]));
    let cnt = 0;
    let max = lefted[0];
    lefted.push(Number.MAX_SAFE_INTEGER);
    for(const progress of lefted){
        if(progress<=max) {
            cnt++;
        }else{
            answer.push(cnt);
            cnt = 1;
            max = progress;
        }
    }
    return answer;
}
```

<br>
