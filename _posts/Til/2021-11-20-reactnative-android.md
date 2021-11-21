---
# []는 카테고리 명 title을 적지 않으면 파일명이 올라간다.
title: "리액트 네이티브 실행 시 안드로이드 에러"
#excerpt는 description
excerpt: "useEffect시 state의 값에 대한 문제"
# 대괄호 안에 들어가는 카테고리 : 목차
categories:
  - "Til"
tags:
  - [React,JavaScript]
# toc는 우측 테이블
toc: true
# 우측 테이블 내용이 따라 오는것 
toc_sticky: true
# 우측 테이블 제목
toc_label: "오늘의 문제점 해결"

toc_icon: "cookie-bite"
# 처음 작성날짜
date: 2021-11-20
# 수정날짜
last_modified_at: 2021-11-20
---

### ERROR : react-native :app:installDebug FAILED 메세지가 나오면서 실행 에러

`ERROR : react-native :app:installDebug FAILED`

[해결 참조링크 : stackoverflow](https://stackoverflow.com/questions/37500205/react-native-appinstalldebug-failed)

- [ ]  앱 실행 시 안드로이드 화면에서 검정 화면이 나온다면?

⇒ 안드로이드 스튜디오 → AVD 매니저 → 해당디바이스 맨 우측 누르고 `wipe data` 선택

- [ ]  java.lang.NoClassDefFoundError: Could not initialize class org.codehaus.groovy.vmplugin.v7.Java7 에러

⇒  원인 : **gradle 버전이 낮아서 나타나는 문제**

⇒해결방법 : 해당 프로젝트의 안드로이드로 이동 → /gradle/wrapper/gradle-wrapper.properties로 이동하고, 

`distributionUrl=https\://services.gradle.org/distributions/gradle-6.3-bin.zip`

gradle 버전을 6.3이상으로 설정하고 다시 gradle 번들 `./gradlew clean build` 을 해준다.

빌드 후 다시 루트 폴더로 와서 run-android