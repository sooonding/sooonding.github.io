---
# []는 카테고리 명 title을 적지 않으면 파일명이 올라간다.
title: "rn에서 styled-component 사용하기"
#excerpt는 description
excerpt: "리액트 네이티브에서 styled-component를 적용하기"
# 대괄호 안에 들어가는 카테고리 : 목차
categories:
  - "reactNative"
tags:
  - [reactNative]
# toc는 우측 테이블
toc: true
# 우측 테이블 내용이 따라 오는것 
toc_sticky: true
# 우측 테이블 제목
toc_label: "rn에서 styled-component 사용하기"

toc_icon: "cookie-bite"
# 처음 작성날짜
date: 2022-02-08
# 수정날짜
last_modified_at: 2022-02-08
---


### 설치

---

```jsx
npm i styled-components
```

### 1. 스타일 적용하기 - Tagged Template Literals

---

```jsx
import styled from 'styled-components/native';

const 컴포넌트이름 = styled.컴포넌트`
	스타일 코드
`
// 실제 코드
const Container = styled.View`
  flex: 1;
  background-color: red;
  align-items: center;
  justify-content: center;
`;
```

### 2. 스타일 적용하기 - 스타일 재사용(CSS)

---

```jsx
import {css} from 'styled-components/native';

const 스타일이름 = css`
	스타일코드
`

const 컴포넌트이름 = styled.컴포넌트`
	${스타일이름}
	스타일 코드
`

//실제 코드
import styled, {css} from 'styled-components/native';

const cssText = css`
  font-size: 20px;
  font-weight: bold;
`;

const StyledText = styled.Text`
  ${cssText}
  color: blue;
`;

```

### 3. 스타일 적용하기 - 스타일 재사용(일반적인 상속)

---

```jsx
import styled from 'styled-components/native';

const StyledText = styled.Text`
  font-size: 20px;
  font-weight: bold;
  color: blue;
`;

*괄호안에 동일하게 사용될 스타일 컴포넌트를 넣어줍니다.*
const ErrorText = styled(StyledText)`
  color: yellow;
`;
```

### 활용하기

---

### 1. props

- `styled-components` 를 사용하면 `props`를 이용하여 style을 변경할 수 있습니다.

```jsx
const StyledText = styled.Text`
  font-size: 20px;
  font-weight: bold;
  color: ${props => (props.name === 'red' ? 'black' : 'yellow')};
`;

const App = () => {
	const name = 'red';
	<StyledText name={name}>{name}</StyledText>
}
```

### 2.attrs

- 컴포넌트 스타일 외에도 다양한 스타일을 변경할 수 있습니다.(예 : `해당 컴포넌트의 속성`)
- `styld.컴포넌트.attrs({ 속성 설정 })`스타일 코드``

```jsx
*일반적인 예시*
const App = () => {
	const name = 'red';
	<StyledInput placeholder='입력해주세요' placeholderTextColor='#fff' />
}

*styled-component를 사용한 예시*

const StyledInput = styled.TextInput.attrs({
	placeholder:'입력해주세요',
	placeholderTextColor:'#fff' 
})`
	border-width: 200px;
`

const App = () => {
	const name = 'red';
	<StyledInput/>
}
```

### 3.ThemeProvider

- “미리 정의된 값들을 지정해서 해당 값을 사용할 수 있게 해줍니다.”

```jsx
import styled,{ThemeProvider} from 'styled-components/native';

*ThemeProvier를 사용한 예시*

const StyledText = styled.Text`
	font-size: 100px;
	color: ${props => props.theme.textColor};
	background0color: ${({theme}) => theme.bg}
`

const App = () => {
	const theme = {textColor: '#009600',bg: '#e3e3e3'};
	<ThemeProvider>
		<StyledText/>
	</<ThemeProvider>
}
```

### 실제로 사용해보기

---

- App.js
    
    ```jsx
    import styled, {css, ThemeProvider} from 'styled-components/native';
    import Input from './src/components/Input';
    
    const Container = styled.View`
      flex: 1;
      background-color: ${({theme}) => theme.bgColor};
      align-items: center;
      justify-content: center;
    `;
    
    const lightTheme = {
      inputColor: '#111111',
      inputBorder: '#111111',
      bgColor: '#e3e3e3',
    };
    
    const darkTheme = {
      inputColor: '#e3e3e3',
      inputBorder: '#e3e3e3',
      bgColor: '#111111',
    };
    
    const App = () => {
      const [toggle, setToggle] = useState(true);
      return (
        <ThemeProvider theme={toggle ? lightTheme : darkTheme}>
          <Container>
            <StatusBar style="auto" />
            <Switch value={toggle} onValueChange={prev => setToggle(prev)} />
            <Input placeholder="zza" />
          </Container>
        </ThemeProvider>
      );
    };
    ```
    

- Input.js
    
    ```jsx
    //   prop를 전달받기에 축약형으로 text로 전달
    const StyledInput = styled.TextInput.attrs(({placeholder}) => ({
      placeholder: placeholder || '입력해주세요',
      placeholderColor: '#111111',
    }))`
      padding: 20px;
      font-size: 20px;
      border: 1px solid ${({text}) => (text ? 'blue' : 'red')};
    `;
    
    const Input = ({placeholder}) => {
      const [text, setText] = useState('');
      return (
        <StyledInput
          onChangeText={e => setText(e)}
          text={text}
          placeholder={placeholder}
        />
      );
    };
    ```