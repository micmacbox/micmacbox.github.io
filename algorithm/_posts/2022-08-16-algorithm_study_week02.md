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

# ❔ 프린터

[문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/42587)

문제 이해하는것도 어려웠다.. 내 이해력 어쩔..ㅋㅋ
처음 풀어보고 두번째는 좀 예쁘게 풀어보려고 애썼는데 가독성이 좋은지는 모르겠다

## 첫번째 문제풀이

```
function solution(priorities, location) {
    let answer = [];
    let _priorities = priorities, _location = location, max = Math.max(...priorities);
    while(true){
        const cur = _priorities.shift();

        if(_location===0 && max === cur){
            answer.push(cur);
            break;
        }
        if(cur === max){
            answer.push(cur);
            max = Math.max(...priorities);
        }else{
            _priorities.push(cur);
        }
        _location--;
        if(_location ==-1) _location = _priorities.length-1;
    }

    return answer.length;
}
```

## 두번째 문제풀이

```
function solution(priorities, location) {
  let answer = 0;
  let _priorities = priorities.map((x, i) => ({ priority: x, loc: i }));

  while (true) {
    const max = Math.max(...Array.from(_priorities.values(), (x, i) => x.priority));
    const cur = _priorities.shift();

    if (cur.loc === location && cur.priority === max) {
      answer++;
      break;
    }
    if (cur.priority === max) answer++;
    else _priorities.push(cur);
  }
  return answer;
```

<br>

# ❔ 다리를 지나는 트럭

[문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/42583)

문제가 너무 난해해서 이해하는데 힘들었다.

다리 위에 올라와있는 트럭들의 총 무게를 구해 다음 트럭이 진입 할 수 있는지 확인했고 모든 트럭이 진입하였을때 다리길이를 더해주었다.

## 문제풀이

```
function solution(bridge_length, weight, truck_weights) {
  let time = 0;
  let ing = [];

  while (truck_weights.length) {
    const curWeight =ing.slice(0, bridge_length-1).reduce((acc, cur) => acc += cur, truck_weights[0]);

    if (curWeight <= weight) ing.push(truck_weights.shift());
    else ing.push(0);
    if(ing.length === bridge_length) ing.shift();
    time++;
  }
  return time += bridge_length;
}

```

<br>
