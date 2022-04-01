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
