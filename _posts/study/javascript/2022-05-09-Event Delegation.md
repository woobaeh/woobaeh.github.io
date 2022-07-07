---
layout: post
title: 이터레이션 프로토콜
description: >

sitemap: false
hide_last_modified: true
categories:
  - study
  - javascript
---



* toc
  {:toc .large-only}

iterable protocol 은 순회가 가능하기 위해서 따라야 하는 규격(인터페이스)


이 규격을 따르면 for...of, spread 연산자에서 사용 가능

기본적인 자료 구조로는 Array, String, Map, Set 이 있다.

어떤 객체이던지 순회가 가능하려면 Iterable 프로토콜을 따라야 한다

{
  [Symbol.iterator](): Iterator 프로토콜
}

iterable : 순회가 가능한
iterator : 반복자

{
  next(): 다음값
}

```js
var someString = "hi";
typeof someString[Symbol.iterator];          // "function"

var iterator = someString[Symbol.iterator]();
iterator + "";                    // "[object String Iterator]"
iterator.next();                  // { value: "h", done: false }
iterator.next();                  // { value: "i", done: false }
iterator.next();                  // { value: undefined, done: true }
```

[mdn](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Iteration_protocols)