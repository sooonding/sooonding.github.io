---
# []는 카테고리 명 title을 적지 않으면 파일명이 올라간다.
title: "에러코드 Cannot read properties of undefined (reading 'getState')"
#excerpt는 description
excerpt: " Next.js에서 Cannot read properties of undefined (reading 'getState') 에러코드가 나는 경우"
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
toc_label: 'next.js 에러코드'

toc_icon: 'cookie-bite'
# 처음 작성날짜
date: 2022-02-16
# 수정날짜
last_modified_at: 2022-02-16
---

Next.js에서 리덕스를 컴포넌트에 연결할 때 해당 문구의 에러가 났습니다.

### 기존코드

```jsx
import React from 'react';
import AppLayout from '../components/AppLayout';
import PropTypes from 'prop-types';
import withRedux from 'next-redux-wrapper';
import { Provider } from 'react-redux';
import reducer from '../reducers';
import { createStore } from 'redux';

const NodeBird = ({ Component, store }) => {
  return (
    <Provider>
      <AppLayout>
        <Component />
      </AppLayout>
    </Provider>
  );
};

export default withRedux((initialState, options) => {
  const store = createStore(reducer, initialState);
  return store;
})(NodeBird);
```

- 해당 사항에 대해서 추가한 것이라곤 `next-redux-wrapper` 만 추가되어있었는데, 구글링 결과 `next-redux-wrapper` 버전에따라 Provider의 유무가 달라지는 것을 알게되었습니다.

- 먼저 에러코드 제일 하단에 `You are using legacy implementaion. Please update your code: use createWrapper() and wrapper.withRedux().` 라는 문구가 나오게 되는데, 검색 결과 제가 사용하고 있는 next-redux-wrapper의 버전에서는 해당 Provider를 설정하지 않아도 wrpper에서 Provider까지 자체 래핑을 해준다는 것을 알게 되었습니다.

### 변경된 코드

```jsx
const NodeBird = ({ Component, store }) => {
  return (
    <>
      <AppLayout>
        <Component />
      </AppLayout>
    </>
  );
};
```

- 해당 `Provider` 부분을 지우니 잘 나오게 되었습니다!

### 참고

[인프런 커뮤니티](https://www.inflearn.com/questions/36034)
