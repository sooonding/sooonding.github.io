---
# []는 카테고리 명 title을 적지 않으면 파일명이 올라간다.
title: "useMemo와 useCallback을 알아보자"
#excerpt는 description
excerpt: 
# 대괄호 안에 들어가는 카테고리 : 목차
categories:
  - "react"
tags:
  - [react]
# toc는 우측 테이블
toc: true
# 우측 테이블 내용이 따라 오는것 
toc_sticky: true
# 우측 테이블 제목
toc_label: "useMemo와useCallback"

toc_icon: "cookie-bite"
# 처음 작성날짜
date: 2021-12-02
# 수정날짜
last_modified_at: 2021-12-02
---



### useMemo와 useCallback 전에 알아야 하는 개념들

1. 함수형 컴포넌트는 단지 `jsx`를 반환하는 함수일 뿐이다.
2. 컴포넌트가 렌더링 된다는건 해당 함수(컴포넌트)를 호출하여 실행하고 있다는 것을 말한다.
3. 컴포넌트는 "자신의 `state`가 변경"되거나 부모에서 받아온 `props`가 변경될 때마다 리 렌더링이된다. 

### 그럼 부모의 값은 무조건 렌더링이 되야해?!

그래서 렌더링 성능 최적화를 위해서 `useMemo`와 `useCallback` 이 나온 것!

`useMemo`는 "특정 value"를 재사용 할 때 사용하고, `useCallback`은 특정 "함수"를 재상용 하기위해 나온 친구들이다.

### useMemo

useMemo는 리액트 공식 사이트에 따르면 **"메모이제이션 된 값만 반환한다"**라고 한다(사실 이 말을 제대로 알았다면 글을 쓰지 않았을 것이다...)

**메모이제이션이란?** 

- 컴퓨터가 동일한 계산을 반복해야할 때, 이전에 계산한 값을 메모리에 저장함으로써 동일한 계산의 반복수행을 **"제거" 하여** 속도를 빠르게 하는 기술

참고 : [https://ko.wikipedia.org/wiki/메모이제이션](https://ko.wikipedia.org/wiki/%EB%A9%94%EB%AA%A8%EC%9D%B4%EC%A0%9C%EC%9D%B4%EC%85%98)

component는 state값이 "변경"될 때마다 리렌더링을 한다라고 했다.

부모에서 받아온 name과 age라는 값이 있다고 예를 들어보자 name과 age는 자식 컴포넌트에서 서로 다른 함수에서 따로 사용되고 가공되는 역할을 한다. 그렇다면 name만 변경되는 함수를 실행하는데 굳이 age도 다시 불러서 값이 계산되어질 이유는 없지 않을까?

useMemo가 바로 이러한 불필요한 연산되는 값을 잡아준다! 

`useMemo`를 사용하면 `dep`에 넘겨준 값이 **"변경"**되었을때만 메모이제이션된 값을 다시 계산한다.

**부모 컴포넌트<App>**

```jsx
import { useState } from "react";
import CreateUser from "./CreateUser";
import "./styles.css";
import UserList from "./UserList";

export default function App() {
  console.log("부모");
  const [name, setName] = useState("");
  const [age, setAge] = useState("");

  // 기존 유저 이름 변경함수
  const onChangeName = (e) => {
    const data = e.target.value;
    console.log("data:", data);
    setName(data);
  };

  //기존 유저 이름 변경 함수
  const onChangeAge = (e) => {
    const age = e.target.value;
    console.log("age:", age);
    setAge(age);
  };

  return (
    <div className="App">
      <CreateUser
        name={name}
        age={age}
        onChangeAge={onChangeAge}
        onChangeName={onChangeName}
      />
    </div>
  );
}
```

**자식 컴포넌트<CreateUser>**

```jsx
import react, { useMemo } from "react";

const setNameTag = (name) => {
  console.log("이름 태그");

  return `이름은 : ${name}`;
};

const setAgeTag = (name) => {
  console.log("나이 태그");

  return `나이는 : ${name}`;
};

const CreateUser = ({ name, age, onChangeName, onChangeAge }) => {
  // 일반적인 props로 받은 값
  // const hiName = setNameTag(name);
  // const hiAge = setAgeTag(age);

  // useMemo를 적용
  const hiName = useMemo(() => setNameTag(name), [name]);
  const hiAge = useMemo(() => setAgeTag(age), [age]);

  return (
    <div>
      <h1>{hiName}</h1>
      <h1>{hiAge}</h1>
      <div>
        <input type="text" onChange={(e) => onChangeName(e)} />
      </div>

      <div>
        <input type="text" onChange={(e) => onChangeAge(e)} />
      </div>
    </div>
  );
};

export default CreateUser;
```

**결과**

[화면 기록 2021-12-01 오후 1.51.39.mov](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/532f07f6-5269-4d4b-b811-37b8a3ba4991/화면_기록_2021-12-01_오후_1.51.39.mov)

실행 결과를 보면 처음에 부모에서 실행되는 콘솔과 name,age 를 실행하는 함수의 콘솔이 모두 재 렌더링이 된다. 

**useMemo를 적용한 코드**

```jsx
const CreateUser = ({ name, age, onChangeName, onChangeAge }) => {
  // 일반적인 props로 받은 값
  // const hiName = setNameTag(name);
  // const hiAge = setAgeTag(age);

  // useMemo를 적용
  const hiName = useMemo(() => setNameTag(name), [name]);
  const hiAge = useMemo(() => setAgeTag(age), [age]);

  return (
    <div>
      <h1>{hiName}</h1>
      <h1>{hiAge}</h1>
      <div>
        <input type="text" onChange={(e) => onChangeName(e)} />
      </div>

      <div>
        <input type="text" onChange={(e) => onChangeAge(e)} />
      </div>
    </div>
  );
};

export default CreateUser;
```

**결과**

![final.gif](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c32ac705-28b1-4113-bf8a-13120edcb5b0/final.gif)

useMemo를 사용하여 해당 state의 변화된 디펜던시만 계산을 하고 변경되지 않은 state의 값은 저장된 값을 사용하여 더 이상 콘솔이 찍히지 않는것을 볼 수 있다!

### useCallback

React에서 컴포넌트가 렌더링 될때에 컴포넌트 안에서 선언된 함수들도 새로 생성이 된다. 그말은 재렌더링 되면 함수도 계속 새로 생성이 된다!

memo는 값을 기억하고 반환하지만 useCallback은 함수를 기억하고 반환한다.

**코드**

```jsx
const Quiz = () => {
  const [string, setString] = useState("");
  const [list, setList] = useState([]);

  const changeHandler = useCallback((e) => {
    setString(e.target.value);
  }, []);

  const add = useCallback(() => {
    const newList = list.slice();
    newList.push(string);
    setList(newList);
  }, [string, list]);

  const sum = useCallback((list) => {
    console.log("실행중입니다..!");
    let stringSum = "";
    for (let value of list) {
      stringSum += value + " ";
    }
    return stringSum;
  }, []);

  const result = useMemo(() => sum(list), [list, sum]);

  return (
    <div>
      <input type="text" onChange={changeHandler} />
      <button onClick={add}>TEXT</button>
      {result}
    </div>
  );
};
```

`changeHandler` 함수는 두번째 인자가  빈 배열로 되어있어 "최초 렌더링 시에만 함수가 생성되고 이후에는 생성되지 않는다. `add` 함수의 경우에는 `string`과 `list`가 **변경될 때에만 함수를 재생성한다.**

`changeHandler,add` 둘 다 string을 사용하는데 어디에는 의존성 배열안에 값을 넣고 어디는 왜 넣지 않을까?

이 이유는 `useMemo` , `useCallback` 이나 동일하게 **해당 함수 안에서 state를 사용할때 반드시 두번째 인자 배열안에 추가 시켜야 하기 때문이다.** `changeHandler` 함수는 해당하는 state를 사용하지 않고 변경만 했으므로 배열에 추가하지 않지만, `add` 함수는 `list` 와 `string` 을 사용하기 때문에 디펜던시에 추가합니다.


### 참고

[nunaung님의 깃헙](https://gist.github.com/ninanung/767ca722befa8b0affe51ffa0064296b)
