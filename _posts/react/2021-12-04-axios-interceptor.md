---
# []는 카테고리 명 title을 적지 않으면 파일명이 올라간다.
title: "Axios를 사용해보자"
#excerpt는 description "발췌부분 설정하면 이 글이 들어가고, 설정하지 않는다면 글의 첫 문단이 들어가게됨"

excerpt: "Axios interceptor 활용하기"
# 대괄호 안에 들어가는 카테고리 : 목차
categories:
  - "axios"
tags:
  - [axios]
# toc는 우측 테이블
toc: true
# 우측 테이블 내용이 따라 오는것 
toc_sticky: true
# 우측 테이블 제목
toc_label: "Axios interceptor"

toc_icon: "cookie-bite"
# 처음 작성날짜
date: 2021-12-06
# 수정날짜
last_modified_at: 2021-12-06
---

### Axios란?

Axios는 "브라우저 환경이나 node 환경에서 사용할 수 있는 "HTTP 클라이언트"라고 한다. 또한
Axios 공식 github에서 보면 **요청 또는 응답을 처리하거나 탐지하기 전에 가로챌 수 있는 것**이라고 설명이 되어있다.

### Axios 사용하기
```jsx
import axios from "axios"

// 인스턴스 생성
const request = axios.create({
  baseURL: process.env.REACT_APP_API_URL,
});

// 요청 타임아웃 설정
request.defaults.timeout = 2500;
```
Axios 인스턴스는 `create()`를 통해 생성할 수 있다. 생성한 인스턴스는 "다른 서버에게 요청을 보낼 수 있는 메서드를 가지고 있는데, 크게 두가지로 나뉜다.

1. `request()` 메서드를 사용하는 방법 
2. `get(), post()`등 HTTP Method 명과 동일한 이름으로 된 메서드를 이용하는 방법

### insterceptor 알아보기

Axios는 **HTTP 요청과 응답을 가로채는 interceptor**를 지원한다. Axios 인스턴스는 "요청과 응답에 대해" 각각 **interceptor 관리자를 가지고 있고** 이 관리자들은 "요청이나 응답을 가로챌 때 실행할 핸들러를 관리합니다.

#### Request interceptor

`axios.interceptor.request.use(onFulfilled,onRejected)`

요청에 대한 interceptor는 "요청을 보내기 직전에" 가로챈다. 프로젝트에서 사용할 때는 공통적으로 사용하는 헤더 같은 것을 직전에 추가할 때 많이 쓰인다고 한다.

`onFulfilled` 핸들러에서 에러가 나게되면 onRejected 핸들러가 실행되고 요청은 이루어지지 않고 즉시 종료된다.


#### Response interceptor

`axios.interceptor.response.use(onFulfilled,onRejected)`

응답에 대한 interceptor는 "상대 서버에서 응답을 받은 **직후에** 가로챈다.
상대방 서버에서 받은 응답이 성공적이라면 onFulfilled 핸들러를 실행할 것이고, 상대방 서버에서 받은 응답이 실패한다면 onRejected가 실행된다.

#### axios interceptor 사용하기

```jsx

import Axios from "axios";

const axios = Axios.create();

axios.interceptors.request.use(
  (config) => {
    console.log("요청: OnFulfilled");
    return config;
  },
  (error) => {
    console.log("요청: OnRejected1");
    return Promise.reject(error);
  }
);

axios.interceptors.response.use(
  (response) => {
    console.log("응답: OnFulfilled");
    return response;
  },
  (error) => {
    console.log("응답: OnRejected");
    return Promise.reject(error);
  }
);

// 실패를 위해 존재하지 않는 경로로 요청
axios.get("url 응답").then((response) => {
  console.log("GET: Data");
});
```

**실행결과**

```jsx
요청: OnFulfilled
응답: OnRejected
```
Axios가 요청한 API가 정상적으로 동작하기 때문에, 요청과 응답에 등록된 핸들러 가운데 `outFulilled`만 실행되었습니다.

**실패 케이스**

```jsx
import Axios from 'axios'

const axios = Axios.create()

axios.interceptors.request.use(
    (config) => {
      console.log('요청: OnFulfilled')
      return config
    },
    (error) => {
      console.log('요청: OnRejected')
      return Promise.reject(error)
    }
)

axios.interceptors.response.use(
    (response) => {
      console.log('응답: OnFulfilled')
      return response
    },
    (error) => {
      console.log('응답: OnRejected')
      return Promise.reject(error)
    }
)

// 실패를 위해 존재하지 않는 경로로 요청
axios.get('url 실패 url')
    .then((response) => {
      console.log('GET: Data')
    })
```
**실행결과**

```jsx
 요청: OnFulfilled
 응답: OnRejected
```
응답이 실패로 돌아오면 `onRejected` 핸들러가 실행된다.



[발췌 : 직방 기술 블로그](https://medium.com/zigbang/%EC%9A%B0%EB%A6%AC-axios%EC%97%90%EA%B2%8C-%EB%8B%A4%EC%8B%9C-%ED%95%9C-%EB%B2%88-%EA%B8%B0%ED%9A%8C%EB%A5%BC-%EC%A3%BC%EC%84%B8%EC%9A%94-a7b32f4f2db2)




