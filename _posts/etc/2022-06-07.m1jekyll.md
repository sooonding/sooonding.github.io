---
# []는 카테고리 명 title을 적지 않으면 파일명이 올라간다.
title: 'm1 jekyll 세팅하기'
#excerpt는 description "발췌부분 설정하면 이 글이 들어가고, 설정하지 않는다면 글의 첫 문단이 들어가게됨"

excerpt: '기존 맥 환경에서 되던 jekyll을 m1에서도 구동 가능하게 변경하자'
# 대괄호 안에 들어가는 카테고리 : 목차
categories:
  - 'etc'
tags:
  - ['jekyll']
# toc는 우측 테이블
toc: true
# 우측 테이블 내용이 따라 오는것
toc_sticky: true
# 우측 테이블 제목
toc_label: 'm1 jekyll 세팅하기'

toc_icon: 'cookie-bite'
# 처음 작성날짜
date: 2022-06-07
# 수정날짜
last_modified_at: 2022-06-07
---

## jekyll m1 세팅하기

기존 인텔맥에서 동작하였던 git blog가 m1 환경에서 동작되지 않아
동작 순서를 간단하게 작성
<br/>
<br/>
### 루비 설치하기

`brew install rbenv ruby-build`

<br/>

### 인스톨 가능한 버전 확인

`rbenv install -l`

<br/>

### 인스톨 수동 설치

`rbenv install {원하는 버전}`

`rbenv global 2.7.5 rbenv rehash`

### gem 으로 번들러 설치시 에러가 나는 경우

```jsx
gem install bundler
ERROR:  While executing gem ... (Gem::FilePermissionError)
    You don't have write permissions for the /Library/Ruby/Gems/2.6.0 directory.
```

system Ruby를 사용하고 있기 때문에 권한이 없다는 에러이다.

\***\*rbenv의 PATH를 추가하여 해결하자\*\***

1. .zsh을 열어준다.

   `code ~/.zshrc`

2. 아래 두줄을 추가

   ```jsx
   export PATH={$Home}/.rbenv/bin:$PATH && \
   eval "$(rbenv init -)"
   ```

3. 변경한 내용을 저장

   `source ~/.zshrc`

4. 다시 번들러를 설치한다.

   `gem install bundler`

### \***\*Jekyll을 설치\*\***

`gem install jekyll`

### 로컬 서버 연결

```jsx
	bundle exec jekyll server
```
<br/>
### config를 수정하는 경우

---

    bundle exec jekyll server 다시 커맨드를 쳐야 새로고침이 된다.

<br/>
### 수정이 끝나면?

---

기존과 동일하게 git push 하면 된다.

<br/>

### 참조

[danalog 블로그 ](https://danaing.github.io/etc/2022/03/14/M1-mac-jekyll-setting.html)
