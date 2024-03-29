---
# []는 카테고리 명 title을 적지 않으면 파일명이 올라간다.
title: '에러코드 Failed to load config prettier/react to extend from.'
#excerpt는 description
excerpt: '기존 prettier 셋팅한 상태에서 npm-start가 에러'
# 대괄호 안에 들어가는 카테고리 : 목차
categories:
  - 'error'
tags:
  - ['error', 'til']
# toc는 우측 테이블
toc: true
# 우측 테이블 내용이 따라 오는것
toc_sticky: true
# 우측 테이블 제목
toc_label: '셋팅시 prettier에러의 경우'

toc_icon: 'cookie-bite'
# 처음 작성날짜
date: 2022-02-17
# 수정날짜
last_modified_at: 2022-02-17
---

### 발생

- 기존에 `npm start` 를 했을 때 구동이 잘 되던 소스코드가 **Failed to load config "prettier/react"**란 에러 메시지를 남기며 동작하지 않는 문제가 발생.

### 해결

구글링 검색 결과 나의 문제점과 가장 비슷한 상황을 보게 되었습니다.

해당 멘션의 글을 인용하자면 version8.0부터는 모든 구성이 하나로 병합되어 “prettier/react”는 더이상 존재하지 않는다고 하였습니다.

솔루션에 따르자면

1. eslint-config-prettie의 버전을 8.0이나 최신 버젼으로 업데이트를 하고
2. extends에서 “prettier/\*”을 제거합니다.

```jsx
//아래와 같은 기존 코드에서
{
  "extends": [
    "some-other-config-you-use",
    "prettier",
    "prettier/react"
  ]
}

// 해당 소스코드처럼 변경해줍니다.
{
  "extends": [
    "some-other-config-you-use",
    "prettier"
  ]
}
```

**참고**

[https://github.com/prettier/eslint-config-prettier/issues/2](https://github.com/prettier/eslint-config-prettier/issues/2)
