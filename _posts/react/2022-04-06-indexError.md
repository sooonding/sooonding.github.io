---
# []는 카테고리 명 title을 적지 않으면 파일명이 올라간다.
title: 'React DOM.render error'
#excerpt는 description
excerpt: 'React18 버전으로 변경되면서 RootRender에서 에러가 나는 경우'
# 대괄호 안에 들어가는 카테고리 : 목차
categories:
  - 'react'
tags:
  - ['react', 'error']
# toc는 우측 테이블
toc: true
# 우측 테이블 내용이 따라 오는것
toc_sticky: true
# 우측 테이블 제목
toc_label: 'React DOM.render error'

toc_icon: 'cookie-bite'
# 처음 작성날짜
date: 2022-04-06
# 수정날짜
last_modified_at: 2022-04-06
---

### 발생

새로 React18 버전으로 CRA를 설치 후 아무것도 변경하지 않았지만 `ReactDOM.render is no longer supported in React 18` 문구와 함께 에러가 나서 기록하게 되었습니다.

### 해결

콘솔에서 해당사항에 대해서 그래도 나름 자세히 알려주는데, 저는 문구 보다는 해당 링크를 통해 들어가서 해결하였습니다. `"ReactDOM.render is no longer supported in React 18. Use createRoot instead."` 해당 링크의 타이틀을 보자면 위와 같은 문구가 있었는데, 말 그대로 리액트 18로 오면서 ReactDOM.render는 사용되지 않고 대신에 `createRoot`라는 새로운 api를 사용하라고 하였습니다. 공홈이 친절하게도 아래와 같은 코드도 알려 줍니다.

```javascript
// Before
import { render } from 'react-dom';
const container = document.getElementById('app');
render(<App tab="home" />, container);

// After
import { createRoot } from 'react-dom/client';
const container = document.getElementById('app');
const root = createRoot(container);
root.render(<App tab="home" />);
```

### 느낀점
