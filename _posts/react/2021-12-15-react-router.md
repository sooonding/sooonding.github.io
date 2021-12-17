---
# []는 카테고리 명 title을 적지 않으면 파일명이 올라간다.
title: 'React Router를 알아보자'
#excerpt는 description
excerpt: React Router의 변화와 복습을 진행
# 대괄호 안에 들어가는 카테고리 : 목차
categories:
  - 'React'
tags:
  - [React]
# toc는 우측 테이블
toc: true
# 우측 테이블 내용이 따라 오는것
toc_sticky: true
# 우측 테이블 제목
toc_label: 'React Router'

toc_icon: 'cookie-bite'
# 처음 작성날짜
date: 2021-12-15
# 수정날짜
last_modified_at: 2021-12-15
---

## Router? 그거 왜 쓰는건데?

HTML은 기존 페이지의 유지가 아닌 전체적인 HTML의 페이지가 다른 HTML로 교체되는 반면, React Routing을 이용하게 되면 기존의 페이지는 그대로 유지한 상태에서 라우팅을 할 수 있기 때문에 성능이 좋다!

설치 `npm i react-router-dom`

- 코드 구조

```jsx
import React from 'react';
import { BrowserRouter as Router, Route, Redirect, Switch} from 'react-router-dom'
import Home from 'Routes/Home'
import Tv from 'Routes/Tv';

export default () => {
  return (
    <Router>
      <Switch>
        <Route exact path="/" component={Home} />
        <Route  path="/tv" component={Tv} />
        <Route  path="/tv/popular" render={() => <h1>popular</h1> } />
        <Redirect from='*' to='/' />
      </Switch>
    </Router>
  )

```

### Redirect

- 일치하는 Route가 없으면 지정한 path 경로로 가게 만들어 준다.

```jsx
<Router>
  <Switch>
    <Route exact path="/" component={Home} />
    <Route path="/tv" component={Tv} />
    <Route path="/tv/popular" render={() => <h1>popular</h1>} />
    <Redirect from="*" to="/" />
  </Switch>
</Router>
```

### Switch

- 한번에 오직 하나의 Route만 Render하게 해준다.
- 안에 있는 경로가 맞으면 해당 컴포넌트를 보여준다고 생각하자.

```jsx
<Router>
  <Switch>
    <Route exact path="/" component={Home} />
    <Route path="/tv" component={Tv} />
    <Route path="/tv/popular" render={() => <h1>popular</h1>} />
  </Switch>
</Router>
```

### exact

- exact 속성은 정확한 path값이 일치하는 것만 나올 수 있도록 해준다.
- exact 속성이 없으면 / 블라블라 ~로 시작되는 동일한 패스값을 가진 애들이 같이 나오게 된다.

### Link

- react Router에서 제공하는 태그로 a 태그를 대체하여 사용할 수 있다.
- a href 를 대체하는 것은 Link `to` 를 사용하면 된다.

### withRouter

- withRouter 함수는 HOC이다.
  라우터에 의해서 호출된 컴포넌트가 아니더라도 match,location,history 객체에 첩근을 할 수 있도록 해준다. 컴포넌트 안에서 props를 활용할 수 있게 해준다.
  Router가 아닌 컴포넌트에게 Router 특성을 부여한다.
  다른 것들은 무조건 link를 사용해서 해당 컴포넌트를 렌더링 하지만, withRouter를 사용하면, 굳이 Link 필요 없이도 화면 렌더링이 가능해진다.
  아래 코드를 봐도 컴포넌트를 withRouter로 감싸고 매개변수로 location,match,history를 받는 걸 알 수 있다.

```jsx
import { withRouter } from 'react-router-dom';

function WithRouterSample({ location, match, history }) {
  return (
    <div>
      <h4>location</h4>
      <textarea value={JSON.stringify(location, null, 2)} readOnly></textarea>
      <h4>match</h4>
      <textarea value={JSON.stringify(match, null, 2)} readOnly></textarea>
      <button onClick={() => history.push('/')}>홈으로</button>
    </div>
  );
}

export default withRouter(WithRouterSample);
```

## Router 내부에서 id에 따른 route 타기(v5)

---

`index.js`

```jsx
<Router>
  <Navbar />
  <Switch>
    <Route exact={true} path="/" component={Home} />
    // ":" 표시를 명확히 보자. // 중첩라우팅 children을 사용하여 더 내부에 있는것을 타게 함
    <Route exact path="/person/:id" children={<Person />} />
    <Route exact path="*" component={Error} />
  </Switch>
</Router>
```

`People.js`

```jsx
<div>
  <h1>People Page</h1>
  {people.map(person => {
    return (
      <div key={person.id} className="item">
        <h4>{person.name}</h4>
        <Link to={`/person/${person.id}`}>Learn More</Link>
      </div>
    );
  })}
</div>
```

### useParams

- 기존에는 <Route>로 사용되지 않은 컴포넌트에서 match,location,history 객체에 접근하려면 `withRouter` Hoc로 컴포넌트를 감싸줘야 했습니다.
- v5.1부터는 `useParams` hook이 추가되면서 손쉽게 객체에 접근이 가능해졌다.

```jsx
import { useParams } from 'react-router';

export default function CardPost() {
  const { slug } = useParams();
}
```

## 실행 예제

### 버튼 클릭으로 다른 페이지로 이동하기

1. **history.push를 이용하는 방법(비추천)**

```jsx
// 상위
<BrowserRouter>
  <nav>
    <Link to="/home">Home</Link>
    <Link to="/profile">Profile</Link>
  </nav>
  <Switch>
    {/* 해당하는 path값이 없을때는 path에 배열or스트링으로 해당 주소를 넣어준다. 예시 <Route path={["/users/:id", "/profile/:id"]}> */}
    <Route exact path={['/', '/home']} component={Home} />
    <Route path="/profile" component={Profile} />
  </Switch>
</BrowserRouter>;

// 하위

import React from 'react';

const Profile = ({ value, history }) => {
  return (
    <div>
      <h1>Profile</h1>
      <button
        onClick={() => {
          history.push('/home');
        }}
      >
        go to home
      </button>
    </div>
  );
};

export default Profile;
```

Route에 해당 component를 추가하고 웹에서 보게되면 아래와 같은 그림으로 해당 값을 볼수가 있다.

![스크린샷 2021-12-15 오전 10.48.24.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3af43818-7f6c-4a36-8073-dc8ee306f48c/스크린샷_2021-12-15_오전_10.48.24.png)

하위 코드에 우리가 웹에서 본 `history`의 값을 `push`하여 넣게 되면 해당 path로 이동하게 된다. 하지만 해당 방법(`<Route/>안에 component를 넣는 방법`)에 대해서는 **공식 홈페이지에서는 추천하지 않는다**고 한다. 왜냐하면 재렌더의 문제가 발생할 수 있다고 한다.

1. **router hook을 사용하는 방법**

1번 케이스의 경우 `<Route>` 의 컴포넌트에 해당 컴포넌트를 "직접"넣어 불러와서 해당 컴포넌트의 props의 value를 받아올 수 있었다. 하지만 렌더의 문제점 때문에 쓰지 않기로 하여 자식 컴포넌트로 해당 컴포넌트를 내리게 되면 `props` 또한 접근할 수 없을것이다. 그럼 어떻게 해결할까?

![아까와는 다르게 해당 props의 value값이 나타나지 않는다.](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/deb5c658-04ef-4dad-8c34-c4dc61a5d981/스크린샷_2021-12-15_오전_11.41.35.png)

아까와는 다르게 해당 props의 value값이 나타나지 않는다.

```jsx
<BrowserRouter>
  <nav>
    <Link to="/home">Home</Link>
    <Link to="/profile">Profile</Link>
  </nav>
  <Switch>
    {/* 컴포넌트로 전달하지 않고 자식 컴포넌트로 전달 */}
    <Route exact path={['/', '/home']}>
      <Home />
    </Route>
    <Route path="/profile">
      <Profile />
    </Route>
  </Switch>
</BrowserRouter>
```

**useHistory()의 등장**

useHistory는 router hook의 api 중 하나로 해당 라우터의 history에 접근할 수 있게 해준다!

```jsx
import React from 'react';
import { useHistory } from 'react-router-dom';

const Home = () => {
	** useHistory로 인하여 해당 밸류값에 접근! **
  const history = useHistory();
  console.log(history, 'Hook');
  return (
    <div>
      <h1>Home</h1>
      <button
        onClick={() => {
          history.push('/profile');
        }}
      >
        go to profile
      </button>
    </div>
  );
};
```

## 끝난줄 알았지..? Router v6의 등장

### React-router v6

2021 하반기까지는 v6가 베타 버전이었기 때문에 v5를 잘 쓰고 있었지만 더 이상은 미룰 수 없기에 (이제까지는 npm으로 설치할 때 `@6`를 명시하였지만 이제는 자동으로 v6로 변경된다!)변경 사항들을 정리하고 적용해 보도록 하자.

### 1. 대폭 작아진 번들 사이즈

v5이전보다 훨씬 가벼워 졌다고 한다. 실제로 크기가 많이 줄었는데 이는 reactRouter만 쓰면 모르겠지만 많은 라이브러리를 사용하니 업데이트를 하는 하나의 이유가 될 수 있다.

### 2. Switch → routes로 변경

기존에 `Route` 를 감싸는 부모요소인 `Switch` 가 `Routes`로 변경되었다.

```jsx
{
  /* Switch => Routes로 변경 */
}
<Routes>
  <Route exact path="/" element={<Home />} />
</Routes>;
```

### 3. exact 옵션의 삭제

v6에서는 `Route` 옵션이 없어졌다. 그 이유는 모든 경로가 정확히 일치하기 때문이다. 만약 하위 경로가 있어 더 많은 URL을 일치 시키려면 `*` 를 사용하여 매칭시킬 수 있다.

```jsx
<Route path="option/*" />
```

### 4. component,render → element로 변경

v6에서는 `element` 속성을 통해 컴포넌트를 바로 넣어줄 수 있도록 변경되었다.

```jsx
//v5
<Switch>
	<Route path='/login' component={Login}/>
</Switch>

//6
<Routes>
  <Route path="/login" element={<Login />} />
</Routes>
```

### 5. Redirect 컴포넌트 제거

v6에서는 더 이상 `<Redirect>` 컴포넌트가 v6버전에서부터는 지원을 하지 않는다.

#### 참고

[React-router 공홈](https://reactrouter.com/)
[jaeme dev](https://www.jaeme.dev/react-router-v6/)
