---
# []는 카테고리 명 title을 적지 않으면 파일명이 올라간다.
title: 'nextjs에서 styledComponent 사용시 className 에러'
#excerpt는 description
excerpt: 'nextjs에서 styledComponent 사용시 스타일이 적용이 안되는 에러를 만날 경우'
# 대괄호 안에 들어가는 카테고리 : 목차
categories:
  - ['nextJs', 'react']
tags:
  - ['nextJs', 'react', 'error']
# toc는 우측 테이블
toc: true
# 우측 테이블 내용이 따라 오는것
toc_sticky: true
# 우측 테이블 제목
toc_label: 'styled-component 적용 에러'

toc_icon: 'cookie-bite'
# 처음 작성날짜
date: 2022-06-22
# 수정날짜
last_modified_at: 2022-06-22

published: false
---

NextJS에서 styled-components를 적용했을 때 뜨는 경고입니다.
말 그대로 className이 달라서 생기는 경고인데요.

NextJS는 초기 렌더링만 서버가 담당(SSR)하고 그 이후에는 서버를 거치지 않은 채 내부 라우팅을 이용해 페이지가 이동되면서 브라우저에서 렌더링(CSR)을 하게 됩니다.

첫 화면 로딩시에는 SSR로 렌더링하면서 오류가 발생하지 않지만 그 이후 부터는 CSR로 렌더링하면서, 서버에서의 클래스명과 클라이언트에서 클래스명이 달라져서 생기는 오류입니다.

해결 방법
`babel-plugin` 설치
npm i babel-plugin-styled-components 2. 프로젝트 루트 경로에 .babelrc 파일 생성 후 내용 작성하기

```javascript
{
   "presets": ["next/babel"],
   "plugins": ["babel-plugin-styled-components"]
}
추가로 babel-plugin-styled-components에 옵션을 줄 수도 있습니다.
{
   "presets": ["next/babel"],
   "plugins": [
      [
        "babel-plugin-styled-components",
        {
           ssr: true,
           displayName: true,
           preprocess: false
        }
      ]
   ]
}
```
