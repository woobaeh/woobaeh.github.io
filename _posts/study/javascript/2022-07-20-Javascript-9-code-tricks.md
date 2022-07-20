---
layout: post
title: 자바스크립트 코드 트릭
description: >

sitemap: false
hide_last_modified: true
categories:
  - study
  - javascript
---

- toc
  {:toc .large-only}

### 1. 구조 분해 할당을 이용한 변수 swap
ES6의 구조 분해 할당 문법을 사용하여 두 변수를 swap 할 수 있습니다.
```
let a = 50, b = 100;
[a, b] = [b, a];
console.log(a, b); // 100 50
```
### 2. 배열 생성으로 루프 제거하기
보통 단순히 범위 루프를 돌고 싶다면 다음과 같이 코드를 작성합니다.
```
let sum = 0;
for (let i = 5; i < 15; i += 1) {
    sum += i;
}
```
만약 범위 루프를 함수형 프로그래밍 방식으로 사용하고 싶다면 배열을 생성해서 사용할 수 있습니다.
```
const sum = Array
    .from(new Array(5), (_, k) => k + 5)
    .reduce((acc, cur) => acc + cur, 0);
```
### 3. 배열 내 같은 요소 제거하기
Set을 이용할 수 있습니다.
```
const names = ['Bae', 'Kim', 'Park', 'Bae', 'Kim'];
const uniqueNamesWithArrayFrom = Array.from(new Set(names));
const uniqueNamesWithSpread = [...new Set(names)];
```
### 4. Spread 연산자를 이용한 객체 병합
두 객체를 별도 변수에 합쳐줄 수 있습니다.
```
const person = {
    name: 'Bae Hyeonwoo',
    familyName: 'Bae',
    givenName: 'Hyeonwoo'
};
```
```
const company = {
    name: 'Code. Inc.',
    address: 'Seoul'
};
```
```
const BaeSunHyoup = { ...person, ...company };
console.log(baeHyeonwoo);
// {
//   address: "Seoul"
//     familyName: "Bae"
//   givenName: “Hyeonwoo”
//   name: "Code. Inc." // 같은 키는 마지막에 대입된 값으로 정해진다.
// }
```
### 5. &&와 || 활용
&&와 ||는 조건문 외에서도 활용될 수 있습니다.
```
/// ||
// 기본값을 넣어주고 싶을 때 사용할 수 있습니다.
// participantName이 0, undefined, 빈 문자열, null일 경우 'Guest'로 할당됩니다.
const name = participantName || 'Guest';

/// &&
// flag가 true일 경우에만 실행됩니다.
flag && func();

// 객체 병합에도 이용할 수 있습니다.
const makeCompany = (showAddress) => {
  return {
    name: 'Code. Inc.',
    ...showAddress && { address: 'Seoul' }
  }
};
console.log(makeCompany(false));
// { name: 'Code. Inc.' }
console.log(makeCompany(true));
// { name: 'Code. Inc.', address: 'Seoul' }
```
6. 구조 분해 할당 사용하기
객체에서 필요한 것만 꺼내 쓰는 것이 좋습니다.
```
const person = {
    name: 'Bae Hyeonwoo',
    familyName: 'Bae',
    givenName: 'Hyeonwoo'
    company: 'Code. Inc.',
    address: 'Seoul',
}

const { familyName, givenName } = person;
```
객체 생성시 키 생략하기
객체를 생성할 때 프로퍼티 키를 변수 이름으로 생략할 수 있습니다.
```
const name = 'Bae Hyeonwoo';
const company = 'Code';
const person = {
  name,
  company
}
console.log(person);
// {
//   name: 'Bae Hyeonwoo'
//   company: 'Code',
// }
```
### 7. 비구조화 할당 사용하기
함수에 객체를 넘길 경우 필요한 것만 꺼내 쓸 수 있습니다.
```
const makeCompany = ({ name, address, serviceName }) => {
  return {
    name,
    address,
    serviceName
  }
};
const Code = makeCompany({ name: 'Code. Inc.', address: 'Seoul', serviceName: 'Present' });
```
### 8. 동적 속성 이름
ES6에 추가된 기능으로 객체의 키를 동적으로 생성 할 수 있습니다.
```
const nameKey = 'name';
const emailKey = 'email';
const person = {
  [nameKey]: 'Bae Hyeonwoo',
  [emailKey]: 'kciter@naver.com'
};
console.log(person);
// {
//   name: 'Bae Hyeonwoo',
//   email: 'kciter@naver.com'
// }
```
### 9. !! 연산자를 사용하여 Boolean 값으로 바꾸기
!! 연산자를 이용하여 0, null, 빈 문자열, undefined, NaN을 false로 그 외에는 true로 변경할 수 있습니다.
```
function check(variable) {
  if (!!variable) {
    console.log(variable);
  } else {
    console.log('잘못된 값');
  }
}
check(null); // 잘못된 값
check(3.14); // 3.14
check(undefined); // 잘못된 값
check(0); // 잘못된 값
check('Good'); // Good
check(''); // 잘못된 값
check(NaN); // 잘못된 값
check`(5); // 5`
```

[코딩테스트 광탈 방지 A to Z : JavaScript](https://school.programmers.co.kr/learn/courses/13213/lessons/91068)