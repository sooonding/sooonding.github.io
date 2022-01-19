---
# []는 카테고리 명 title을 적지 않으면 파일명이 올라간다.
title: 'git 명령어 모음'
#excerpt는 description "발췌부분 설정하면 이 글이 들어가고, 설정하지 않는다면 글의 첫 문단이 들어가게됨"

# excerpt: ''
# 대괄호 안에 들어가는 카테고리 : 목차
categories:
  - 'git'
tags:
  - [git]
# toc는 우측 테이블
toc: true
# 우측 테이블 내용이 따라 오는것
toc_sticky: true
# 우측 테이블 제목
toc_label: 'git 명령어 모음'

toc_icon: 'cookie-bite'
# 처음 작성날짜
date: 2022-01-18
# 수정날짜
last_modified_at: 2022-01-18

# 공개/비공개 전환
published: false
---

## Promise

- "언젠가" 끝날 무언가를 합시다. 라고 정의할 떄 `Promise`를 사용합니다.
- "구독"의 개념과 비슷하다고 보면 좋을 거 같습니다. `Promise`는 또한 자바스크립트 객체로 되어있습니다.
- promise는 자바스크립트의 비동기 처리에 사용되는 "객체" 입니다.

`promise`의 객체 형태는 아래의 코드처럼 만들 수 있습니다.

```js
let promise = new Promise(function (resolve, reject) {
  // executor
});
```

`new Promise`에 전달되는 함수는 `executor(실행 함수)`라고 명칭합니다. 실행함수는`new Promise`가 만들어질 때 자동으로 실행되는데, 결과를 최종적으로 반환하는 코드를 포함합니다.
`resolve, reject`는 자바스크립트에서 자체적으로 제공하는 콜백입니다. 다만, 인수로 넘겨준 콜백 중 하나는 **"반드시 호출해야 합니다"**

- `resolve`: 일이 "성공적"으로 끝난 경우 결과를 나타내는 `value`와 함께 호출
- `reject`: 에러가 발생 시 에러 객체를 나타내는 `error`와 함께 호출

`new Promise` 생성자가 반환하는 Promise의 객체는 아래와 같은 프로퍼티를 가지고 있습니다.

- `state` : 처음 상태는 `pending`인 상태에서 `resolve`가 호출이 되면 `fulfilled`(약속이 이행된 프로미스), `reject`가 호출되면 `error`로 변합니다.

```js
let promise = new Promise(function (resolve, reject) {
  // 프로미스가 만들어지면 실행함수는 자동으로 실행

  // 1초 후에 성공 신호가 전달되면서 result는 fine이 됩니다.
  setTimeout(() => resolve('fine'), 1000);
});
```

### 프로미스는 성공 혹은 실패만 합니다.

실행함수는 `resolve`나 `reject`중 하나를 "반드시" 호출해야 됩니다. 이 변경된 상태는 더 이상 변하지 않고
처리가 끝난 프로미스의 `resolve`나 `reject`를 호출하면 무시됩니다.

## then

`.then`은 프로미스의 기본 메서드로 함수 실행을 해주는 메서드입니다.

```js
// 문법
promise.then(
  function(result){ /* 결과를 다루는 함수 */ }
  function(error){ /* 에러를 다루는 함수 */ }
)


let promise = new Promise(function (resolve, reject) {
  setTimeout(() => resolve('done'), 1000);
});

// resolve 함수는 .then의 첫번째 함수(인수)를 실행합니다.
promise.then(result => console.log(result)) // 1초후 done을 출력
```

## promise를 좀 더 쉽게 사용하는 async,await

### async 함수

function 앞에 `async`를 붙이면 항상 프로미스를 반환합니다. 만약 프로미스가 아닌 것은 프로미스로 감싸 반환합니다.

### await

`await` 키워드는 `async` 함수 안에서만 동작합니다. await은 프로미스가 처리 될 때까지 기다리다가 결과는 그 이후 반환합니다. `await`은 promise.then 보다 가독성이 좋고 쓰기도 쉽습니다.

### 참고

[https://github.com/jeonghwan-kim/git-usage](https://github.com/jeonghwan-kim/git-usage)
