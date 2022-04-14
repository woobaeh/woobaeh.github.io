---
layout: post
title: 깊은 복사(Deep Copy)와 얕은 복사(Shallow Copy)
description: >

sitemap: false
hide_last_modified: true
categories:
  - til
---

얕은 복사 : 바로 아래 단계의 값만 복사

깊은 복사: 내부의 모든 값들을 하나하나 전부 복사

```jsx
var user = { name: "damon", gender: "male" };
var copyObject = function (target) {
  var result = {};
  for (var prop in target) {
    result[prop] = target[prop];
  }
  return result;
};

var user2 = copyObject(user);
```

위 코드에서는 얕은 복사를 수행 한 모습입니다.

얕은 복사인 경우에는 **중첩된 객체**에서 참조형 데이터가 저장된 프로퍼티를 복사할 때 그 주솟값만 복사합니다.

그러면 해당 프로퍼티에 대해 원본과 사본 모두 동일한 참조형 데이터의 주소를 가리키게 됩니다.

사본을 바꾸면 원본이 , 원본을 바꾸면 사본도 바뀝니다.

```jsx
var user = {
  name: "hongchul",
  urls: {
    portfolio: "http://github.com/abc",
    blog: "http://blog.com",
    facebook: "http://facebook.com/abc",
  },
};

var user2 = copyObject(user);

user2.name = "Jung";
console.log(user.name === user2.name); // false

user.urls.portfolio = "http://portfolio.com";
console.log(user.urls.portfolio === user2.urls.portfolio); //true

user2.urls.blog = "";
console.log(user.urls.blog === user2.urls.blog); // true;
```

⇒ 위 코드에서 사본인 user2 의 name 프로퍼티를 바꿔도 user의 name 프로퍼티는 바뀌지 않습니다.

하지만 user.urls.portfolio, user2.urls.portfolio의 경우 바뀐 것이 확인 됩니다.

**즉 user객체에 직접 속한 프로퍼티에 대해서는 복사해서 완전히 새로운 데이터가 만들어진 반면, 한 단계 더 들어간 urls의 내부 프로퍼티들은 기존 데이터를 그대로 참조 하는 것이죠.**

⇒ 이런 현상이 발생하지 않게 하려면 user.urls 프로퍼티에 대해서도 불변 객체로 만들 필요가 있습니다.

```jsx
var user2 = copyObject(user);
user2.urls. copyObject(user.urls); 내부의 프로퍼티들을 복사

user.urls.portfolio = 'http://portfolio.com';
console.log(user.urls.portfolio === user2.urls.portfolio); //false

user2.urls.blog = '';
console.log(user.urls.blog === user2.urls.blog); //false

```

객체의 프로퍼티 중에서 그 값이 기본형 데이터인 경우에는 그대로 복사하면 되지만 참조형 데이터는 다시 그 내부의 프로퍼티들을 복사해야 합니다.

이 과정을 참조형 데이터가 있을 때마다 재귀적으로 수행해야만 비로소 깊은 복사가 되는 것입니다!

_이 개념을 바탕으로 copyObject 함수를 깊은 복사 방식으로 고친 코드_

```jsx
var copyObjectDeep = function (target) {
  var result = {};
  for (var prop in target) {
    if (typeof target === "object" && target !== null) {
      result[prop] = copyObjectDeep(target[prop]);
    } else {
      result = target;
    }
  }
  return result;
};
```

객체인 경우에는 내부 프로퍼티들을 순회하며 copyObjectDeep 함수를 재귀적으로 호출하고, 객체가 아닌 경우에는 target를 지정합니다. 이제 원본과 사본 서로 완전히 다른 객체를 참조하게 되어 어느 쪽의 프로퍼티를 변경하더라도 다른 쪽에 영향을 주지 않습니다.

**JSON을 이용해서 간단하게 깊은 복사를 하는 방법**

```jsx
var copyObjectViaJSON = function (target) {
  return JSON.parse(JSON.stringify(target));
};

var obj = {
  a: 1,
  b: {
    c: null,
    d: [1, 2],
    func1: function () {
      console.log(3);
    },
  },
  func2: function () {
    console.log(4);
  },
};

var obj2 = copyObjectViaJSON(obj);
```

객체를 JSON 문법으로 표현된 문자열로 전환했다가 다시 JSON 객체로 바꾸는 방법.

이 경우에는 함수나, 숨겨진 프로퍼티등과 같이 JSON으로 변경할 수 없는 프로퍼티들은 모두 무시합니다. httpRequest로 받은 데이터를 저장한 객체를 복사할 때 등 순수한 정보를 다룰 때 활용하기 좋은 방법입니다.
