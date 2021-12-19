---
# []는 카테고리 명 title을 적지 않으면 파일명이 올라간다.
title: "[codeWars]Highest and Lowest"
#excerpt는 description "발췌부분 설정하면 이 글이 들어가고, 설정하지 않는다면 글의 첫 문단이 들어가게됨"

excerpt: 
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
toc_label: "Highest and Lowest"

toc_icon: "cookie-bite"
# 처음 작성날짜
date: 2021-12-17
# 수정날짜
last_modified_at: 2021-12-17
---

### 문제

In this little assignment you are given a string of space separated numbers, and have to return the highest and lowest number.

```js
Examples
highAndLow("1 2 3 4 5");  // return "5 1"
highAndLow("1 2 -3 4 5"); // return "5 -3"
highAndLow("1 9 3 4 -5"); // return "9 -5"
```

Notes
1. All numbers are valid Int32, no need to validate them.
2. There will always be at least one number in the input string.
3. Output string must be two numbers separated by a single space, and highest number is first.


### 문제해설

문맥을 보고 해당 반환값을 보자면 해당 문자열의 인자를 넘겨 줄 때 "가장 큰 숫자와 가장 작은 숫자"를 포함한 문자열로 반환하라는 문제로 보입니다. 문제를 푸는 솔루션 코드를 한번 봅시다.

### 해결방법

메서드를 이용하여 푸는 방법과 일반 for문으로 푸는 두 가지 케이스로 풀어보았습니다.

```js
const highAndLow1 = (numbers) => {
  const numArr = numbers.split(" ");
  const result = numArr.sort((a,b) => a-b);
  return `${result[result.length-1]} ${result[0]}`
}
```
-> sort 메서드로 간단하게 풀 수 있다. sort로 작은수부터 나열하고 리터럴 문법으로 반환하면 끝!

```js
function highAndLow(numbers){
  const numArr = numbers.split(" ");
  let max = numArr[0]; //배열 중 하나의 값을 지정
  let min = numArr[0]; //배열 중 하나의 값을 지정

  for(let i = 0; i < numArr.length; i++) {
    if(Number(numArr[i]) > max) {
      max = Number(numArr[i]);
    }
    if(Number(numArr[i]) < min) {
      min = Number(numArr[i]);
    }
    result = `${max} ${min}`
  }
  return result;
}
```

-> for문안에서 값을 넣는 위치 때문에 많이 생각 외로 많이 헤맸습니다. 처음에 for문 안에서 해당 값을 지정해야 된다고 생각하여 넣었는데 엉뚱한 값이 자꾸 나와서 생각해 보니, **for 문을 돌면서 해당 변수의 값도 계속 변하기 때문에** 엉뚱한 값이 나올 수밖에 없었습니다. 간단하게 생각하면 받아온 인자의 값 중 첫 번째 인자 값을 지정만 해서 루프를 돌아 리턴을 시켜주면 되는 것이었습니다.

### 다른 사람의 풀이

```js
const highAndLow1 = (numbers) => {
  numbers = numbers.split(' ').map(Number);
  return Math.max.apply(0, numbers) + ' ' + Math.min.apply(0, numbers);
}
```
**"???? map을 저렇게 사용한다고?!"**

![ㅁㅁㅁ](https://images.velog.io/images/sooonding/post/cdb5ecdd-9685-4e02-ac3c-36f97ae3169a/%E1%84%8D%E1%85%A1%E1%86%AF.jpeg)

해당 풀이를 보면서 `map`의 매서드를 활용하는 방법을 새로이 느꼈습니다.
`split`으로 각 배열로 쪼개놓은 배열값을 `Number`를 이용하여 해당 배열 타입을 넘버로 변경되는 방법을 채득했습니다.
그리고 직접 짠 코드와 달랐던 점은 `apply` 메서드의 활용이었습니다.


### 배운점

1. 매서드를 사용하지 않고 for문으로만 짜는게 내가 생각 한것보다 조금 오래걸린걸 보면 좀 더 for문으로만 활용하여 문제를 풀어야 되는점
2. map(Number)를 활용하여 해당 배열 타입을 넘버로 일괄 적용하는 방법을 알게 되었습니다.