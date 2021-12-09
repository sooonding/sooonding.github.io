---
# []는 카테고리 명 title을 적지 않으면 파일명이 올라간다.
title: "MMS 이미지 전송하기(feat: naver-SENS)"
#excerpt는 description
excerpt:
# 대괄호 안에 들어가는 카테고리 : 목차
categories:
  - "javaScript"
tags:
  - [javaScript]
# toc는 우측 테이블
toc: true
# 우측 테이블 내용이 따라 오는것
toc_sticky: true
# 우측 테이블 제목
toc_label: "MMS 이미지 전송하기(SENS)"

toc_icon: "cookie-bite"
# 처음 작성날짜
date: 2021-12-09
# 수정날짜
last_modified_at: 2021-12-09
---

### naver SENS를 이용한 MMS 이미지 파일 전송하기

업무를 하면서 naver SENS를 이용한 MMS 전송방법에 대해 어려움이 있어, 이렇게 블로깅으로 정리하려고 한다.

기술 스택으로 view를 만드는 템플릿은 Pug를 이용하였고 동적인 기능 부분은 js와 jquery를 이용하였다.

html과 js&jquery를 이용하여 코드를 먼저 작성해보자.

```jsx
$("#file").change(function(){
        var result;
        fileList = $("#file")[0].files;
        console.log(fileList,'메타정보');
        let files = fileList[0];
        $('#fileText').innerHTML = fileList[0].name;
        $('#fileText').val(fileList[0].name)
        getBase64(files)
      });
```

- 코드를 보면 해당 파일을 담고있다.(fileList는 input 태그이며 type이 "file"로 실행되는 태그이다.) 해당 파일을 `getBase64` 라는 함수안에 인자로 주는 로직이다.

```jsx
const getBase64 = (file) => {
      //reader 변수는 FileReader()라는 
      let reader = new FileReader();
      reader.readAsDataURL(file);
      reader.onload = function(){
        $('#fileItem').val(reader.result);
      };
      reader.onerror = function (error) {
        console.log(error)
      }
    }
```
- 이어서 위에 코드를 보면 `input` 파일로 받은 인자를 인코딩하고 







