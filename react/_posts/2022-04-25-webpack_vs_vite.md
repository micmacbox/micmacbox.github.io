---
layout: post
title: webpack vs vite
categories: react
tags: [javascript, react]
date: 2022-04-25 15:32 +0800
toc: false
---

자바스크립트 모듈 시스템을 사용하기 위해 webpack, rollup, parcel 같은 다양한 번들러들이 나타났다.
이런 번들러들을 사용할 때 큰 프로젝트일수록 트랜스파일의 시간이 많이 들었다.
vite는 es모듈을 활용하는 자체 개발 서버를 가지고 있어 웹팩보다 훨씬 더 빠른 속도를 제공하며 rollup을 사용하여 빠른 빌드가 가능하다.

## webpack vs vite

webpack의 bundle based dev server
![](/assets/img/webpack_devserver.png)

vite의 native ESM based dev server
![](/assets/img/vite_devserver.png)
