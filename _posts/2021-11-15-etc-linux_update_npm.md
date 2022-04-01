---
layout: post
title: Ubuntu 리눅스 최신버전 node.js, npm 설치
categories: etc
tags: [Ubuntu, Linux]
date: 2021-11-15 15:32 +0800
# toc: true
---

### 1. PPA 등록

```
$ curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
```

(14.x가 아닌 다른 버전을 선택하고 싶으면 14를 원하는 버전으로 바꿔주자)

### 2. apt install로 nodejs, npm 설치

```
$ sudo apt install nodejs
$ sudo apt install npm
```

### 3. 여러 빌드 툴이 포함된 build-essential 설치

```
$ sudo apt install build-essential
```
