---
layout: post
title: git rebase
categories: git
tags: [git]
date: 2022-01-26 15:32 +0800
# toc: true
---

## Rebase란?

리베이스는 커밋의 베이스를 똑 떼서 다른 곳으로 붙이는 것이다.

## Merge와 Rebase 차이점

코드 충돌이 발생했을 때, 내 브랜치로 먼저 머지하고, 충돌을 해결한 다음 다시 풀 리퀘스트를 보내면 충돌이 나지 않는다. 하지만 풀 리퀘스트에 충돌을 해결하느라 생긴 머지 커밋이 생긴다. **리베이스는 깔끔하게 변경한 부분만 풀 리퀘스트를 보낼 수 있는 방법이다.**

## Rebase 원리

1.  HEAD(feature)와 대상 브랜치(master)의 공통 조상을 찾는다.
2.  공통 조상 이후에 생성한 커밋들을 대상 브랜치(master) 뒤(마지막 커밋)로 재배치한다.
3.  **리베이스 완료된 커밋들은 체크섬이 바뀐 새로운 커밋이다.**

## Rebase 방법

1.  `git checkout feature`
2.  `git rebase master`
3.  `git checkout master`
4.  `git merge feature` : 한 줄이 되었기 때문에 빨리 감기 병합을 수행한다.
