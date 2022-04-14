---
layout: post
title: "[알고리즘 풀이] 소수 만들기"
categories: algorithm
tags: [javascript, algorithm]
date: 2022-04-15 23:49 +0800
# toc: true
---

[소수 만들기](https://programmers.co.kr/learn/courses/30/lessons/12977)

## 문제 설명

주어진 숫자 중 3개의 수를 더했을 때 소수가 되는 경우의 개수를 구하려고 합니다. 숫자들이 들어있는 배열 nums가 매개변수로 주어질 때, nums에 있는 숫자들 중 서로 다른 3개를 골라 더했을 때 소수가 되는 경우의 개수를 return 하도록 solution 함수를 완성해주세요.

##### 제한사항

- nums에 들어있는 숫자의 개수는 3개 이상 50개 이하입니다.
- nums의 각 원소는 1 이상 1,000 이하의 자연수이며, 중복된 숫자가 들어있지 않습니다.

---

##### 입출력 예

| nums        | result |
| ----------- | ------ |
| [1,2,3,4]   | 1      |
| [1,2,7,6,4] | 4      |

##### 입출력 예 설명

입출력 예 #1  
[1,2,4]를 이용해서 7을 만들 수 있습니다.

입출력 예 #2  
[1,2,4]를 이용해서 7을 만들 수 있습니다.  
[1,4,6]을 이용해서 11을 만들 수 있습니다.  
[2,4,7]을 이용해서 13을 만들 수 있습니다.  
[4,6,7]을 이용해서 17을 만들 수 있습니다.

## 문제풀이

```
var sums = [];
var answer = 0;

function combination(arr, a, b){
    if(arr.length-2<=a){
        for(let val in sums){
            if(isPrime(sums[val])){
                answer++;
            }
        }
        return;
    }

    if(arr.length-1<=b){
        combination(arr, a+1, a+2);
        return;
    }

    for(let i=b+1; i<arr.length; i++){
        sums.push(arr[a] + arr[b] + arr[i]);
    }
     combination(arr, a, b+1);

}

function isPrime(num) { for(let i = 2; i <= Math.sqrt(num); ++i){ if(num % i == 0) return false; } return true; }

function solution(nums) {
    combination(nums, 0,1);
    return answer;
}
```
