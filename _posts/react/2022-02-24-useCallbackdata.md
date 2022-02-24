---
# []는 카테고리 명 title을 적지 않으면 파일명이 올라간다.
title: 'useCallback을 애매하게 알면 생기는 일'
#excerpt는 description
excerpt: 'useCallback을 사용할 때 제대로 사용하지 않아 벌어진 일을 기록합니다.'
# 대괄호 안에 들어가는 카테고리 : 목차
categories:
  - 'react'
tags:
  - ['react']
# toc는 우측 테이블
toc: true
# 우측 테이블 내용이 따라 오는것
toc_sticky: true
# 우측 테이블 제목
toc_label: 'useCallback 사용시 반성합니다.'

toc_icon: 'cookie-bite'
# 처음 작성날짜
date: 2022-02-24
# 수정날짜
last_modified_at: 2022-02-24
---

### 발생

`me`라는 데이터를 초깃값으로 `null`로 지정한 후, 동작 이벤트가 발생 했을 때 해당 me의 값에 대한 조건으로 기능이 동작해야 하는데 동작되지 않는 문제점이 있었습니다.

### 코드

```jsx
const Example = () => {
  const { me } = useSelector(state => state.user);
  const onSubmitComment = useCallback(() => {
    //NOTE:댓글은 로그인 한 사람만 받을 수 있다. 그렇기에 조건을 걸어줌
    if (!me) {
      alert('로그인을 먼저 하세요');
    }
    dispatch({ type: ADD_COMMENT_REQUEST });
  }, []);

  return (
    <Form onFinish={onSubmitComment}>
      <Form.Item></Form.Item>
      <List header={`${item.Comment ? item.Comment.length : 0} 댓글`}/></List>
    </Form>
  );
};
```

### 해결

`onSubmitComment`라는 함수 안에서 해당 `me`가 null이라 나와서 에러가 나와 진행이 되지 않았습니다.
`useCallback`의 디펜던시를 처음에 비우고 시작하였는데, 생각해 보니 `me`(로그인 시 데이터 값이 들어오는 값)는 초깃값이 null로 들어와 있어 그 해당 null 값으로 만 기억을 합니다. 함수 상태가 바꼈을 경우 디펜던시에 변경되는 값 `me`를 넣어야 null이 아닌 변경된 값을 기억할 수 있기 때문에 디펜던시를 비웠던 것이 문제였습니다.

```jsx
const onSubmitComment = useCallback(() => {
  if (!me) {
    alert('로그인을 먼저 하세요');
  }
  dispatch({ type: ADD_COMMENT_REQUEST });
}, [me]); //NOTE: 이곳에 변경값이 있는 state값을 넣어줘야 합니다.
```

### 느낀점

`useCallback`을 안다고 생각했지만 그냥 스쳐 지나가듯이 해당 코드를 짜면 해당 에러가 났을 때 디버깅하는 시간이 오래 걸리겠구나라는 생각을 하게 되었습니다.
코딩을 할 때 어떠한 기능을 사용을 하게 되는 거라면 면밀히 살펴보고 코드를 짜야겠다는 걸 느끼게 되었습니다.
