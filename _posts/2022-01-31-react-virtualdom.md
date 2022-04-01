---
layout: post
title: React Virtual Dom
categories: react
tags: [javascript, react]
date: 2021-01-31 15:32 +0800
# toc: true
---

# 1. DOM이란

리액트의 핵심개념인 Virtual DOM을 설명하기에 앞서 Document Object Model(DOM)이 무엇인지 알아보자.

MDN의 설명을 보자.

> The **Document Object Model** (**DOM**) is the data representation of the objects that comprise the structure and content of a document on the web.
>
> The Document Object Model (DOM) is a programming interface for HTML and XML documents. It represents the page so that programs can change the document structure, style, and content. The DOM represents the document as nodes and objects. That way, programming languages can connect to the page.
>
> A Web page is a document. This document can be either displayed in the browser window or as the HTML source. But it is the same document in both cases. The Document Object Model (DOM) represents that same document so it can be manipulated. The DOM is an object-oriented representation of the web page, which can be modified with a scripting language such as JavaScript.

요약 하자면 HTML이나 XML 문서들을 자바스크립트같은 프로그래밍 언어로 조작 할 수 있도록 되어 있는 모델이다.

![](https://miro.medium.com/max/972/1*LA6AXbzvC0IQ_d2H8v3NCw.gif)

w3school에서 가져온 html DOM tree of objects

그림으로 보면 조금 더 이해가 쉽다. 저런 DOM모델을 이용하여 자바스크립트가 “document.getElementById”와 같은 코드를 사용하여 html태그를 동작 할 수 있다.

그리고 우리가 보는 페이지에 변화가 있을때마다 DOM은 새로 업데이트 되어야하고 css까지 생각하면 페이지변화는 컴퓨터에게 상대적으로 비싼 작업이다.

# 2. Virtual DOM이란

이제는 버츄얼돔에 대해서 알아보자. 리액트 doc은 이렇게 설명해준다:

> The virtual DOM (VDOM) is a programming concept where an ideal, or “virtual”, representation of a UI is kept in memory and synced with the “real” DOM by a library such as ReactDOM. This process is called [reconciliation](https://reactjs.org/docs/reconciliation.html).

버츄얼돔은 우리가 가상적으로 UI를 저장해두었다가 Real DOM과 연동(sync)시켜주는 프로그래밍 컨셉이다. 공식문서에서 “_reconciliation_”이라는 과정을 통해서 한다.

어떻게 동작 한다는 건지 도대체 글로만 봐서는 알 수가 없다. 직접 코드를 보면서 **_React diffing alrorithm_** 이 어떻게 동작 하는지 보면 너무나 쉽게 이해할 수 있다.

# 3. “Diffing” Algorithm

아까 얘기했던 reconciliation 과정이 어떻게 작동하는지 알아보자. 리액트는 Real DOM과 Virtual DOM을 참고한다. 그리고 “_diffing_” 알고리즘을 이용해 변화가 일어난 DOM 요소만 새로 렌더링을 한다.

직접 코드를 보면서 리액트가 어떻게 작동 하는지, 그리고 어떤 규칙들이 있는지 알아보자.

## 3.1 Elements Of Different Types

상위 root element가 바뀌면 하위 element도 함께 바뀐다.

<div>  
  <Counter />  
</div>  
  
<span>  
  <Counter />  
</span>

위와 같이 상위태그가 div에서 span으로 바뀌게 되면 하위 Counter 컴포넌트 자체는 바뀌지 않았더라도 리액트는 모두 갈아엎고 새로운 DOM을 생성한다.

## 3.2 DOM Elements Of The Same Type

그렇다면 요소(element)는 같지만 속성(attribute)만 바뀌면 어떨까?

<div className="before" title="stuff" />  
  
<div className="after" title="stuff" />

이런 경우엔 리액트는 아까처럼 DOM은 유지시키고 className 요소만 바꾼다. 새로 DOM을 생성하지 않고 효율적으로 렌더링 할 수 있다.

## 3.2 Recursing On Children

하위 요소가 추가 되어지는 경우에도 우리는 생각을 할 필요가 있다.

<ul>  
  <li>first</li>  
  <li>second</li>  
</ul>  
  
<ul>  
  <li>first</li>  
  <li>second</li>  
  <li>third</li> //여기만 추가.  
</ul>

이렇게 세번째 리스트만 추가가 되어진 경우엔 어떨까? 리액트 _diffing_ 알고리즘은 첫번째, 두번째 리스트가 같은 것을 확인했다. 그리고 세번째가 다른걸 확인하였기 때문에 세번째 element만 추가한다.

리액트 덕분에 효율적으로 DOM을 렌더링 할 수 있다. 기존 DOM 방식이었다면 갈아엎고 새로 생성 되어졌을 일이다.

우리가 리액트의 도움을 받는만큼 우리도 리액트를 배려해줘야 하는 경우도 있다. 이렇게 내용은 같지만 순서가 바뀐 경우에는 다르다. 아래와 같은 케이스를 보자.

<ul>  
  <li>Duke</li>  
  <li>Villanova</li>  
</ul>  
  
<ul>  
  <li>Connecticut</li> //Conneticut만 추가되었지만 순서가 첫 번째로 들어감.  
  <li>Duke</li>  
  <li>Villanova</li>  
</ul>

우리의 바램은 리액트가 “선생님, 제가 Conneticut을 생긴것을 찾아냈으니 이것만 추가로 생성하겠습니다.” 이렇게 해주었으면 좋겠지만, 아니다.

첫 번째 리스트가 Duke가 Conneticut으로 바뀐 것을 알아채고 리액트는 li의 모든 요소를 새로 렌더링한다.

## 3.3 Keys

방금과 같은 문제는 key를 이용하면 개선할 수 있다.

<ul>  
  <li key="2015">Duke</li>  
  <li key="2016">Villanova</li>  
</ul>  
  
<ul>  
  <li key="2014">Connecticut</li>  
  <li key="2015">Duke</li>  
  <li key="2016">Villanova</li>  
</ul>

이렇게 각 요소에 key를 지정해주면 순서가 다르더라도 처음부터 렌더링을 하지 않고 변화가 생긴 부분만 렌더링을 해준다.

<li key={item.id}>{item.name}</li>

위와같이 id를 사용하면 효율적으로 좋은 코드를 쓸 수 있다. 그리고 key를 사용하지 않는 경우에 리액트에서 warning 메세지를 보내므로 key를 꼭 사용하자.

# 4. 마무리

마지막으로 버츄얼돔에 대해서 공부하다 인상깊었던 영상을 올린다. 리액트 버추얼돔에 대해서 재밌게 설명해주는 영상이다.

<iframe width="560" height="315" src="https://www.youtube.com/embed/BYbgopx44vo" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

**_출처:_**

MDN DOM: [_https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction_](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction)
React Virtual DOM: [_https://reactjs.org/docs/faq-internals.html#what-is-the-virtual-dom_](https://reactjs.org/docs/faq-internals.html#what-is-the-virtual-dom)
React Reconciliation: [_https://reactjs.org/docs/reconciliation.html_](https://reactjs.org/docs/reconciliation.html)
리액트: Virtual DOM(가상돔, 버추얼돔)이란?: [_https://dj-min43.medium.com/%EB%A6%AC%EC%95%A1%ED%8A%B8-virtual-dom-%EA%B0%80%EC%83%81%EB%8F%94-%EB%B2%84%EC%B6%94%EC%96%BC%EB%8F%94-%EC%9D%B4%EB%9E%80-359c28112048_](https://dj-min43.medium.com/%EB%A6%AC%EC%95%A1%ED%8A%B8-virtual-dom-%EA%B0%80%EC%83%81%EB%8F%94-%EB%B2%84%EC%B6%94%EC%96%BC%EB%8F%94-%EC%9D%B4%EB%9E%80-359c28112048)
