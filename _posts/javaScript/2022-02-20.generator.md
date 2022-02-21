---
# []는 카테고리 명 title을 적지 않으면 파일명이 올라간다.
title: 'generator 알아보기'
#excerpt는 description
excerpt: '자바스크립트를 공부하면서 외면했던 제너레이터를 알아 봅시다.'
# 대괄호 안에 들어가는 카테고리 : 목차
categories:
  - 'javaScript'
tags:
  - ['javaScript']
# toc는 우측 테이블
toc: true
# 우측 테이블 내용이 따라 오는것
toc_sticky: true
# 우측 테이블 제목
toc_label: '제너레이터 알아보기'

toc_icon: 'cookie-bite'
# 처음 작성날짜
date: 2022-02-20
# 수정날짜
last_modified_at: 2022-02-20
---

# 제너레이터

### generator

---

### 제너레이터

- 일반적인 함수를 사용했을 때 예시를 보자
  ```jsx
  function normal() {
    console.log(1);
    console.log(2);
    console.log(3);
  }
  ```
  - 해당 사항은 바로 실행이 되면서 1,2,3이 순차적으로 나올 것입니다. 하지만 제너레이터를 사용하면 중간에서 정지하고 싶을 경우에도 사용할 수 있습니다.
- 제너레이터 사용할 때

  - yield는 중단점 역할을 함 원하는 시점에 yield를 넣으면 함수가 중단
  - `yield*` : 별을 붙이면 iterable 형태로 사용이 가능합니다.

```jsx
function* one() {
  console.log(1);
  console.log(2);
  yield 3;
  console.log(4);
  yield 14;
  yield* '1234'; // 별을 붙이면 iterable(반복 가능한 값)
  /* yield 1,yield 2,yield 3 === yield* [1,2,3]은 동일하다 */
}

var gen = one();

gen.next(); // 1,2,{value:3,done:false}
gen.next(); // 4, {value:14, done:false}
```

### redux-saga에서 generator가 대두되는 이유?

---

- `async-await` 이전에 비동기 코드를 동기 코드처럼 사용하기 위해
- `await` 의 성향을 `yield` 랑 비슷한 맥락이라고 보면 된다. 사실 yield가 `async` `await`보다 더 강력한 기능이 있습니다.
- 비동기를 자유자재로 컨트롤을 할 수 있기 때문에 계속 제너레이터를 사용하는 이유
- 간략히 보자면 `yield` 로 함수를 중단하고 `next` 로 함수를 다시 실행하고 value

### 무한 반복문을 제너레이터로 컨트롤 하기

---

- `yield`가 중단점이 되기 때문에 무한 반복문을 컨트롤을 할 수 있습니다.

```jsx
function* wha() {
  let i = 0;
  while (true) {
    yield i++; // yield가 중단을 해주기 때문에 무한 반복이 되지 않음
  }
}

const gen = wha();

gen.next(); // 1
gen.next(); // 2
gen.next(); // 3
gen.next(); // 4
```

### 참고

[제로초님 블로그](https://www.zerocho.com/category/ECMAScript/post/579b34e3062e76a002648af5)
