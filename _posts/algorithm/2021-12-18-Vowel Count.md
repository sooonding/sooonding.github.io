---
# []는 카테고리 명 title을 적지 않으면 파일명이 올라간다.
title: "[codeWars]VowelsCount"
#excerpt는 description "발췌부분 설정하면 이 글이 들어가고, 설정하지 않는다면 글의 첫 문단이 들어가게됨"

excerpt: "Return the number (count) of vowels in the given string.
We will consider a, e, i, o, u as vowels for this Kata (but not y).
The input string will only consist of lower case letters and/or spaces."

# 대괄호 안에 들어가는 카테고리 : 목차
categories:
  - "algorithm"
tags:
  - [algorithm]
# toc는 우측 테이블
toc: true
# 우측 테이블 내용이 따라 오는것 
toc_sticky: true
# 우측 테이블 제목
toc_label: "VowelsCount"

toc_icon: "cookie-bite"
# 처음 작성날짜
date: 2021-12-18
# 수정날짜
last_modified_at: 2021-12-18
---

### 문제

Return the number (count) of vowels in the given string.
We will consider a, e, i, o, u as vowels for this Kata (but not y).
The input string will only consist of lower case letters and/or spaces.

### 문제해설

인자로 받는 문자열 중 `a,e,i,o,u`가 포함되면 해당 문자열에 카운트를 반환하는 문제
예로 'aei'라는 문자열을 인자로 받는다면, 3이 반환되는 문제

### 해결방법

**case 1**

```js
function getCount(str) {
  let vowelsCount = 0
  const vowels = ["a", "e", "i", "o", "u"]
  for(let char of str) {
      if(vowels.includes(char)) {
          vowelsCount++
      }
  }
  return vowelsCount;
}
getCount('aei')
```
-> 카운트를 세기 위해 vowelsCount 변수를 선언하고 조건에 해당하는 string을 배열로 저장합니다.
`for of`를 이용하여 문자열은 반복하고 `includes` 매서드를 이용해서 받아온 인자의 "각각의 문자"가 포함이 되는지에 대한 조건을 걸어 포함이 된다면 카운트를 늘려줍니다.

**case2**

```js
function getCount2(str) {
  var vowelsCount = 0;
  const arrStr = str.split('');
  const vowels = ["a", "e", "i", "o", "u"]
  arrStr.forEach((el) => {
    for(let i = 0; i < vowels.length; i++){
      if(el === vowels[i]){
        vowelsCount++;
      }
    }
  })
  return vowelsCount;
}
getCount('mxya');
```

-> 해당 받아온 인자를 먼저 `split` 매서드를 이용하여 배열로 담고, 해당 배열을 `forEach`를 이용하여 루프를 돌아 줍니다.
`forEach`를 이용하여 각각의 문자를 받아올 수 있게 되었고 콜백 안에서 조건에 해당하는 배열을 `for`문을 이용해 루프를 돌게 되면 각각 배열에 대해서 비교가 가능 해 집니다.
두 문자열이 같다면 결국 조건배열에 부합하기 때문에 카운트를 늘려서 반환합니다.


### 배운점

`forEach`보다는 `map`을 이용하였는데 배열을 반환하지 않아도 되니 배열을 반복하는 `forEach`를 이용하는 것이 나쁘지 않겠다는 생각을 했습니다.
새롭게 접근하기 위해 `for of`문과`includes`를 이용하는 다양한 접근 방식도 좋았던 거 같습니다.
