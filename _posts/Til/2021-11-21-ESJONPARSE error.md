---
# []는 카테고리 명 title을 적지 않으면 파일명이 올라간다.
title: "npm start 시 code EJSONPARSE 에러"
#excerpt는 description
excerpt: 
# 대괄호 안에 들어가는 카테고리 : 목차
categories:
  - "error"
tags:
  - [npm,error]
# toc는 우측 테이블
toc: true
# 우측 테이블 내용이 따라 오는것 
toc_sticky: true
# 우측 테이블 제목
toc_label: "오늘의 문제점 해결"

toc_icon: "cookie-bite"
# 처음 작성날짜
date: 2021-11-21
# 수정날짜
last_modified_at: 2021-11-21
---

npm start 시에 **`code EJSONPARSE` 에러를 마주하였습니다.** 

### 해결

해당 에러는 `package.json`에서 불필요한 주석이나 콤마가 들어가면 발생하는 오류인거 같다는 자료를 참고하여 

`package.json` 에 들어가 확인해보니 git 충돌로 인한 코드 크래시를 병합하지 않아 발생한 것으로 보여 병합하고 다시 시작 해 보니 해당 화면이 잘 나왔습니다.

### 결론

해당 코드가 발생할 경우에는 `package.json`을 확인해 보자!

참고
[해결 참조링크 : velog(ryong9rrr님)](https://prod.velog.io/@ryong9rrr/npm-error-code-EJSONPARSE)
