---
# []는 카테고리 명 title을 적지 않으면 파일명이 올라간다.
title: 'vscode 상단 컬러 변경하기'
#excerpt는 description "발췌부분 설정하면 이 글이 들어가고, 설정하지 않는다면 글의 첫 문단이 들어가게됨"

excerpt:
# 대괄호 안에 들어가는 카테고리 : 목차
categories:
  - 'etc'
tags:
  - [etc]
# toc는 우측 테이블
toc: true
# 우측 테이블 내용이 따라 오는것
toc_sticky: true
# 우측 테이블 제목
toc_label: 'vscode 상단 컬러 변경하기'

toc_icon: 'cookie-bite'
# 처음 작성날짜
date: 2021-12-19
# 수정날짜
last_modified_at: 2021-12-19
---

vscode 여러가지 프로젝트를 열다보면 작업하던 해당 에디터가 어떤건지 구분하기가 어렵습니다.
잘못된 곳에서 코딩하는것을 방지하고자 상단의 컬러를 변경하는 방법을 알아 보겠습니다.

### 방법

1. vscode settings로 진입
2. Workspace로 가서 Appearance에 Color Customizations의 settings.json을 클릭합니다.

```js
  "workbench.colorCustomizations": {
    "titleBar.activeBackground": "#ffbede", // 사용되고 있는 탭 배경색
    "titleBar.activeForeground": "#ff0080", // 사용되고 있는 텍스트 컬러
    "titleBar.inactiveBackground": "#00a2ff", // 그 외의 에디터 배경색
    "titleBar.inactiveForeground": "#00ff15" // 그 외의 에디터 텍스트 컬러
  },

```

### 참고

[vscode 공식 홈페이지](https://code.visualstudio.com/api/references/theme-color)
