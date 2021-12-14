---
# []는 카테고리 명 title을 적지 않으면 파일명이 올라간다.
title: "Pagination 사용하기"
#excerpt는 description
excerpt:
# 대괄호 안에 들어가는 카테고리 : 목차
categories:
  - "React"
tags:
  - [React]
# toc는 우측 테이블
toc: true
# 우측 테이블 내용이 따라 오는것
toc_sticky: true
# 우측 테이블 제목
toc_label: "Pagination 사용하기"

toc_icon: "cookie-bite"
# 처음 작성날짜
date: 2021-12-15
# 수정날짜
last_modified_at: 2021-12-15
---


### 구현준비

1. react-hooks를 이용
2. 데이터 통신은 Axios를 이용하여 더미 데이터를 불러온다.

**자 이제 준비가 되었으면 코드를 작성해보자**


#### APP.js


App.js에서는 해당 페이지에 관련된 state를 만들고 실행 로직을 작성하였다.
pagination은 결국 해당 페이지와 해당 페이지에서 보여질 숫자를 클릭하여 실행하는 것이기 떄문에
현재 페이지와 페이지당 포스트 갯수에 관련된 state를 작성하였다.
아래 코드를 보자.

```jsx

import axios from "axios";
import { useCallback, useEffect, useState } from "react";
import Pagination from "./components/Pagination";
import Post from "./components/Post";
import "./styles.css";

export default function App() {
  const [posts, setPosts] = useState([]);
  const [loading, setLoading] = useState(false);
  const [currentPage, setCurrentPage] = useState(1); // 현재 페이지
  const [postPerPage, setPostPerPage] = useState(10); // 페이지당 포스트 개수

  const initData = useCallback(async () => {
    setLoading(true);
    try {
      const response = await axios.get(
        "https://jsonplaceholder.typicode.com/posts"
      );
      setPosts(response.data);
      setLoading(false);
    } catch (e) {
      console.log(e); // 에러처리
    }
  }, []);

  useEffect(() => {
    initData();
  }, []);

  // 현재 페이지 가져오기
  const lastPost = currentPage * postPerPage; // 1 * 10 = 10번 포스트
  const firstPost = lastPost - postPerPage; // 10-10 0번 포스트
  const currentPost = posts.slice(firstPost, lastPost); // 0~10번까지 포스트

  // 클릭 이벤트페이지 바꾸는 함수 로직
  const paginate = (pageNum) => setCurrentPage(pageNum);

  return (
    <div className="App">
      <h2>content post</h2>
      <Post posts={currentPost} loading={loading} />
      <Pagination
        postPerPage={postPerPage}
        totalPosts={posts.length}
        paginate={paginate}
      />
    </div>
  );
}
```

#### App.js 코드설명

더미 데이터를 불러오기 위해 useEffect 사용하여 초기 렌더링시에 데이터를 받아오고, 데이터를 `posts` state에 담았다.
각 페이지에 보여주는 post의 갯수를 위해 `lastPost` `firstPost` `currentPost` 변수에 각 해당 포스트를 담는 계산식을 사용하였다. 또한 보여지는 view와 pagination 컴포넌트를 각각 나누었다.

계산식을 이해하기 위해 설명을 곁들이겠다.

```jsx
  lastPost === '마지막포스트'
  firstPost === '첫번째포스트'
  currentPost === '보여줄 데이터에 대한 배열'
  postPerPage === '가용포스트'
  currentPage === '현재페이지'

  첫번째포스트 = 마지막포스트 - 가용포스트;
  마지막포스트 = 현재페이지 * 가용포스트;
  현재포스트 = posts(useEffect로 받아온 포스트 데이터).slice(첫번째포스트,마지막포스트);

  가용포스트를 10으로 보고 현재페이지가 1일 경우 
  마지막 포스트의 값은 1 * 10이 되니 10이 된다.
  첫번째 포트의 경우 마지막포스트가 현재 10이고 가용 포스트의 값이 10이니 뺄셈을 하게 되면 0이 된다.
  이처럼 첫번쨰와 마지막 포스트에 대한 값은 동적으로 받아 올 수 있는 계산식이 완성이 되니
  우리는 해당 포스트(현재 데이터)에 대한 갯수를 잘라서 보여 줄 수 있게 된다.
  받아온 posts를 첫번째 포스트와 마지막 포스트로 잘라내게 되면 해당 데이터만 뿌려서 볼 수 있다!
```



#### Post

```jsx
import React from "react";

const Post = ({ posts, loading }) => {
  if (loading) {
    return <h3>loading...</h3>;
  }
  return (
    <>
      <ul>
        {posts &&
          posts.map((item) => {
            return <li key={item.id}>{item.title}</li>;
          })}
      </ul>
    </>
  );
};

export default Post;
```

#### Post.jsx 컴포넌트 설명

상위 App.js에서 받아온 posts,loading를 이용하여 각각 view단에 보여지는 조건을 걸어주었다.



#### Pagination

```jsx
import React from "react";

// 페이지 번호를 출력하는 컴포넌트
// 페이지네이션 컴포넌트를 따로 만든 이유는 다른 곳에서도 불러와서 사용할 수 있기위함
const Pagination = ({ postPerPage, totalPosts, paginate }) => {

  const pageNumbers = [];
  /* Math.ceil은 소수점 이하를 올림한다. */
  for (let i = 1; i < Math.ceil(totalPosts / postPerPage); i++) {
    console.log(i,'푸시')
    pageNumbers.push(i);
  }
  return (
    <nav>
      <ul>
        {pageNumbers.map((num) => {
          return (
            <li key={num}>
              <a onClick={() => paginate(num)} href="!#">
                {num}
              </a>
            </li>
          );
        })}
      </ul>
    </nav>
  );
};
export default Pagination;
```
#### Pagination.jsx 컴포넌트 설명

실질적으로 페이지의 번호가 나열되어있는 list(pagination)을 보여주는 컴포넌트로
보통 페이지네이션은 다른곳에서도 불러와서 사용할 수 있기 때문에 따로 컴포넌트로 만든다.

페이지에 대한 number를 보여주기 위해 state를 만들고 초깃값으로 빈 배열을 나열한다.
해당 페이지를 나열하여 보여주기 위해 for 문을 이용하였고 해당 반복문을 돌면서 나열할 배열을 `push` 해준다.


### 배운점

해당 page number를 어떻게 뿌릴건가에 대한 생각만 많이 하였는데 **어떻게** 해당 포스트를 잘라내고 보여줄지에 대한 계산을
좀더 명확하고 깔끔하게 배운 거 같다.


#### 참고
[글쓰는 갓디노](https://goddino.tistory.com/218)
