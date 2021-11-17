---
# []는 카테고리 명 title을 적지 않으면 파일명이 올라간다.
title: "computed property(계산된 프로퍼티명)"
#excerpt는 description "발췌부분 설정하면 이 글이 들어가고, 설정하지 않는다면 글의 첫 문단이 들어가게됨"

excerpt: 
# 대괄호 안에 들어가는 카테고리 : 목차
categories:
  - "javaScript"
tags:
  - [javaScript]
# toc는 우측 테이블
toc: true
# 우측 테이블 내용이 따라 오는것 
toc_sticky: true
# 우측 테이블 제목
toc_label: "객체 계산된 프로퍼티명"

toc_icon: "cookie-bite"
# 처음 작성날짜
date: 2021-11-17
# 수정날짜
last_modified_at: 2021-11-17
---

## 설명

ECMAScript2015부터 나온 기능으로 대괄호 `[]`표현식을 허용하고 변수에 담겨있는 값이 객체의 키가 되어 값에 접근할 수 있도록 합니다.

```jsx
let fruit = "lele"

// 아래와 같이 사용하는 이는 변수에 있는 값을 [] 표기법으로 변수를 넣어 가져와
let bag = {
  [fruit]: 123456, 
};

console.log( bag.lele ); // 값 : 5 값에 접근할 수 있다.


let items = ['A','B','C'];

const obj = {
  [items] : "Hello"
}
obj
console.log(obj["A,B,C"]); // 'Hello"
```

### 참고

[MDN-Property accessors](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Property_Accessors)
