---
layout: post
title: 알고리즘 정리
categories: etc
tags: [algorithm]
date: 2022-03-28 15:32 +0800
# toc: true
---

## 섬 둘레 구하기

```
    function solution(maps)
    {
    let answer = 0;
    let row=maps.length;
    let col=maps[0].length;
    for (let i=0; i<row; i++){
    for (let j=0; j<col;j++){
    if (maps[i][j]){
    answer+= (4 - (()=>{
    let cnt = 0;
    if (i>0 && maps[i-1][j]===1){
    cnt++;
    }
    if (j>0 && maps[i][j-1]===1){
    cnt++;
    }
    if (i<row-1 && maps[i+1][j]===1){
    cnt++;
    }
    if (j<col-1 && maps[i][j+1]===1){
    cnt++;
    }
    return cnt;
    })());
    }
    }
    }
    return answer;
    }
```

## 유저가 주문한 음식

유저가 주문한 음식 데이터를 이용해 음식을 가장 다양하게 주문한 유저는 누구인지 알아보려 합니다. 유저는 주문 한 번당 음식 여러 종류를 주문하며, 같은 음식을 여러 번 주문할 수도 있습니다.

예를 들어 음식 주문 데이터가 다음과 같은 경우

["alex pizza pasta", "alex pizza pizza", "alex noodle", "bob pasta", "bob noodle sandwich pasta", "bob steak noodle"]

"alex"는 "pizza", "pasta", "noodle"을 주문했습니다.
"bob"은 "pasta", "noodle", "sandwich", "steak"를 주문했습니다.
따라서 "bob"이 주문한 음식이 총 네 종류로 가장 많습니다.

유저 ID와 각 유저가 주문한 음식이 문자열 형태로 들어있는 배열 orders가 매개변수로 주어질 때, 가장 많은 종류의 음식을 주문한 유저의 아이디를 배열에 담아 return 하도록 solution 함수를 완성해주세요. 만약, 그런 유저가 여러 명이면 해당 유저들을 오름차순으로 정렬해 return 하면 됩니다.

제한사항
1 ≤ orders의 길이 ≤ 200,000
orders의 원소는 음식 주문 데이터가 "유저ID 음식1 음식2 ..." 순서로 들어있습니다.
유저는 한 번에 최대 5개까지 음식을 주문합니다.
유저 ID와 음식 이름은 공백(스페이스 바) 하나로 구분해서 주어집니다.
음식과 음식도 공백(스페이스 바) 하나로 구분해서 주어집니다.
유저 ID와 음식 이름은 길이가 1 이상 10 이하인 문자열이며, 알파벳 소문자로만 이루어져 있습니다.
입출력 예
orders result
["alex pizza pasta", "alex pizza pizza", "alex noodle", "bob pasta", "bob noodle sandwich pasta", "bob steak noodle"]["bob"]
["alex pizza pasta steak", "bob noodle sandwich pasta", "choi pizza sandwich pizza", "alex pizza pasta steak"]["alex", "bob"]
입출력 예 설명
입출력 예 #1
문제 예시와 같습니다.

입출력 예 #2

"alex"와 "bob"은 음식 세 종류를 주문했으며, "choi"는 두 종류를 주문했습니다. 따라서 오름차순 정렬하여 ["alex", "bob"]을 return하면 됩니다.

```
function solution(orders){
	let arr = [], user = {};

	orders.map(order=>{
	        const arr = order.split(" ");
	        if(user[arr[0]]==undefined) user[arr[0]] = [...arr];
	        else user[arr[0]] = [...user[arr[0]], ...arr];
	});

	for(key in user){
	    user[key] = [...new Set(user[key])];
	    arr.push(user[key]);
	}

	let max = Math.max.apply(Math, arr.map(el=>{ return el.length }));
	answer = arr.filter(_=> _.length === max).reduce((acc, _)=>{acc.push(_[0]); return acc;},[]);
	return answer;
}
```

## 좌표평면 위의 로봇

문제 설명
4방향으로 움직일 수 있는 로봇이 좌표평면의 원점에 서 있습니다. 이 로봇에게 알파벳으로 명령을 내려 특정 방향으로 1칸만큼 이동을 시킬 수 있습니다. 로봇은 4방향으로 움직일 수 있으며, U는 위, L은 왼쪽, R은 오른쪽, D는 아래로 이동하라는 명령어입니다. 로봇에게 명령을 하나의 문자열로 묶어서 내릴 수 있습니다. 예를 들어, 로봇에게 다음과 같은 명령을 내렸다고 가정해 봅시다.

URLLDRLR

로봇은 (0,0) → (0,1) → (1,1) → (0,1) → (-1,1) → (-1,0) → (0,0) → (-1,0) → (0,0) 의 순서로 이동하여 원점으로 돌아오게 됩니다. 하지만 우리는 이 명령어 중 연속되는 일부 명령어만을 내려도 원점으로 돌아오게 할 수 있습니다. 그 경우는 다음과 같습니다.

명령어 명령어의 범위(a번째 문자열~b번째 문자열을 의미)
RL [1,2]
RL [5,6]
LR [6,7]
URLLDR [0,5]
URLLDRLR [0,7] ※명령어 전체도 포함
결론적으로 위의 명령어의 전체, 또는 연속되는 일부 명령어 중 로봇이 출발하여 시작지점으로 돌아오는 경우의 수는 5가지입니다.

로봇의 명령어 s가 주어질 때, 경우의 수를 return 하는 solution 함수를 완성해 주세요.

제한사항
'R', 'L', 'U', 'D' 이외에 명령어는 주어지지 않습니다.
명령어 s의 길이 : 1,000 이하의 자연수
입출력 예
s result
"URLLDRLR" 5
"RLDU" 3
"URDLDRULDLRUDLU" 14
입출력 예 설명
입출력 예 #1
문제의 예시와 같습니다.

입출력 예 #2
0번째 문자열부터 1번째 문자열인 "RL", 2번째 문자열부터 3번째 문자열인 "DU", 0번째 문자열부터 3번째 문자열인 "RLDU" 의 3가지 경우가 원점으로 돌아오게 할 수 있습니다. 그러므로 3을 return 하면 됩니다.

입출력 예 #3
위와 같은 방식으로 하면 됩니다.

```
function solution(s){
  const comm = {
    U: [-1, 0],
    D: [1, 0],
    L: [0, -1],
    R: [0, 1]
  };
  let answer = 0;

  answer = s.split('').reduce((acc , command, idx, src)=>{
    let result = src.reduce((_acc, _command, _idx, _src)=>{
      if(_idx<idx){
        return _acc;
      }
      // 1. 기존 좌표값이 없다면 현재 좌표값을 저장한다. (_acc[0])
      // 2. 현재 좌표값과 기존 _acc[0]의 좌표값을 더하여 _acc[0]를 업데이트한다.
      //  2-1. 2의 좌표값이 0,0에 수렴하면 count를 +1 시킨다. (_acc[1])
      // 3. _acc[1]를 반환한다.

      if(_acc[0] == 0){
        _acc[0] = comm[_command]
        return _acc;
      };

      _acc[0] = _acc[0].reduce((__acc, __mat, __idx) => {
        __acc[__idx] = __mat+comm[_command][__idx];
        return __acc;
      }, [0,0]);

      if(_acc[0].toString() === [0,0].toString()){
        _acc[1] +=1;
      }

      return _acc;
    },[0, 0]);
    return acc + result[1];
  },0);

  return answer;
}
```
