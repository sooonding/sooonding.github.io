---
# []는 카테고리 명 title을 적지 않으면 파일명이 올라간다.
title: 'Axios CORS 쿠키 전송하기'
#excerpt는 description
excerpt: 'withCredentials 옵션 알아보기'
# 대괄호 안에 들어가는 카테고리 : 목차
categories:
  - 'library'
tags:
  - ['library', 'til']
# toc는 우측 테이블
toc: true
# 우측 테이블 내용이 따라 오는것
toc_sticky: true
# 우측 테이블 제목
toc_label: 'Axios CORS 쿠키 전송하기'

toc_icon: 'cookie-bite'
# 처음 작성날짜
date: 2023-02-05
# 수정날짜
last_modified_at: 2023-02-05
---

## CORS 허용해도 에러가 나는 현상

<br>
웹 구성시 리액트,뷰,앵귤러 같은 라이브러리|| 프레임워크를 사용하면 프론트 서버를 “따로” 개발하여 사용하게 된다.<br>

만일 클라이언트 서버가 [localhost](http://localhost):3000이고 API서버가 [localhost:4000](http://localhost:4000)면 “서로 같은 host만 사용하지 port번호”는 다른 입장이다. 그렇기에 cors 에러를 마주하게 된다.
<br>

그래서 지책으로 `Access-Control-Allow-origin` 과 같이 헤더에 값을 모두 허용한다고 해도 에러를 마주하게 되는 경우가 있는데 어떻게 해결할까?

## withCredentials 옵션으로 쿠키 보내기

`withCredentials` 옵션은 단어 그대로 “다른 도메인”에 요청을 보낼 때 요청에 인증정보(credential)를 담아서 보낼 지를 결정하는 항목

쿠키나 인증 헤더 정보를 포함시켜 요청하고 싶다면, 클라이언트에서 API 요청 메소드를 보낼 때 `withCredentials` 옵션을 “true”로 설정하면 된다.

또한 인증된 요청을 정상적으로 수행하려면 클라이언트 뿐만 아니라 서버에서도 `Access-Control-Allow-Credentials` 을 true로 인증 옵션을 설정해주어야 한다.

## 정리

- **클라이언트와 서버 둘 다 Credentials부분을 true로 설정**
  1. 표준 CORS요청은 기본적으로 쿠키를 설정하거나 보낼 수 없다.
  2. 프론트 axios요청 할 때, `withCredentials` 부분을 true 로 수동으로 설정하여 CORS 요청값에 쿠키값을 넣어준다.
  3. 마찬가지로 서버도 응답헤더에 `Access-Control-Allow-Credentials` 를 true로 설정해준다.

## 예제 - node

- 클라이언트 로그인 시
    <figure>
      <img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/4701b685-2fef-4ac6-8fe6-83767cf77a21/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230205%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230205T050958Z&X-Amz-Expires=86400&X-Amz-Signature=0e75df31a62c0673ce81350ef9ed1ed3bd76e3b115f71c26f02f8951506e0153&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject">
    </figure>
    axios.post로 작성시 세번째 인자에 withCredentials를 true로 설정

- 서버 API 작업 시
  <figure>
      <img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/58572514-4b01-4bcf-a434-74403bf17d44/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230205%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230205T050846Z&X-Amz-Expires=86400&X-Amz-Signature=70446236b2b326752079cbedc3a8f7b8ddbe940405dae8d91dd7fb99a91cb5d3&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject">
    </figure>
  cors 라이브러리를 이용하여 credentials를 true로 설정해주었다.

**참고**

[블로그-인파](https://inpa.tistory.com/entry/AXIOS-%F0%9F%93%9A-CORS-%EC%BF%A0%ED%82%A4-%EC%A0%84%EC%86%A1withCredentials-%EC%98%B5%EC%85%98#:~:text=withCredentials%20%EC%98%B5%EC%85%98%EC%9D%80%20%EB%8B%A8%EC%96%B4%20%EA%B7%B8%EB%8C%80%EB%A1%9C,%EC%A7%80%EB%A5%BC%20%EA%B2%B0%EC%A0%95%ED%95%98%EB%8A%94%20%ED%95%AD%EB%AA%A9%EC%9D%B4%EB%8B%A4.)
