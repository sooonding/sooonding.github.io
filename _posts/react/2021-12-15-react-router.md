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
last_modified_at: 2022-03-31
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

<p align="center">
  <img src='https://images.velog.io/images/sooonding/post/02310ed7-d4f3-438e-9af4-04a5f7261596/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-12-15%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.48.24.png'>
</p>

하위 코드에 우리가 웹에서 본 `history`의 값을 `push`하여 넣게 되면 해당 path로 이동하게 된다. 하지만 해당 방법(`<Route/>안에 component를 넣는 방법`)에 대해서는 **공식 홈페이지에서는 추천하지 않는다**고 한다. 왜냐하면 재렌더의 문제가 발생할 수 있다고 한다.

1. **router hook을 사용하는 방법**

1번 케이스의 경우 `<Route>` 의 컴포넌트에 해당 컴포넌트를 "직접"넣어 불러와서 해당 컴포넌트의 props의 value를 받아올 수 있었다. 하지만 렌더의 문제점 때문에 쓰지 않기로 하여 자식 컴포넌트로 해당 컴포넌트를 내리게 되면 `props` 또한 접근할 수 없을것이다. 그럼 어떻게 해결할까?

<p align="center">
  <img src='https://images.velog.io/images/sooonding/post/f2db0bd1-4e77-472a-80f0-985d9c4e4f92/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-12-15%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2011.41.35.png'>
</p>

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

### 6. Nested Router의 변화

nested Router는 route안에 있는 또 다른 라우터를 말한다.

- v6에서 구현하는 방법은 이전 버전과는 다르며 2가지의 타입을 가지고 있다.
  1. 부모 route의 마지막에 `/*` 을 적어 해당 route의 내부에서 중첩 렌더가 되게 표시하고
     자식 route를 부모 route element의 내부에 작성하는 방법
  2. `Outlet` (중첩 라우트 사용)을 이용하는 방법으로 Outlet은 부모 경로 요소에서 자식 경로 요소를 렌더링하는데 사용해야 된다. 이를 통해 하위 경로가 렌더링 될 때 중첩된 ui 화면을 표시할 수 있다. 부모라우트가 정확히 일치하면 자식 라우트를 렌더링 하지만 다를 경우 렌더링이 이뤄지지 않음
- 내 블로그 기재는 2번의 방식이 좀 더 직관적이라고 생각되어 outlet을 이용한 코드를 보여준다.

```javascript
// Router.tsx
// 해당 부모 라우트에 자식라우트를 감싸주고,
<Route path=":coinId" element={<Coin />}>
  <Route path="chart" element={<Chart />} />
  <Route path="price" element={<Price />} />
</Route>
// Coins.tsx
// 부모에서 Outlet을 이용하여 중첩 표시를 해준다.
<>
  <OverviewItem>
    <span>Max Supply:</span>
    <span>{priceInfo?.max_supply}</span>
  </OverviewItem>
<Outlet />
</>
```

### 7. useRouteMatch() => useMatch()로 변경

`useRouteMatch()`가 사라지고 `useMatch(url주소)` 로 전환 “현재 위치를 기준으로 지정된 경로에 대한 일치 데이터를 반환합니다.” 덧붙이자면 `useMatch()`의 인자로 url을 넘기면 해당 url과 일치할 경우 url의 정보를 반환하고, 일치하지 않을 경우 `null`을 반환한다.

```javascript
const priceMatch = useMatch('/:coinId/price');
```

### 8. Link의 변경

v5와는 다르게 state속성을 따로 빼서 해당 값을 넣어준다.

```javascript
// 기존 v5
<Link to={{ pathname: "/home", state: state }} />

// v6의 변경된 코드
<Link to="/home" state={state} />
```

타입에 대한 `interface`도 변경되었다. `v6`에서는 제네릭을 지원하지 않아 interface로 해당 타입을 지정할 때 `as`를 사용해준다.

```javascript
interface RouteParams {
  helloId: string;
}

function Hello() {
  //NOTE: 넘겨받은 state는 해당 코인의 값이 들어가있거고 그것을 접근하는것이 "useLocation"
  const location = useLocation();
  const name = location.state as RouteParams;
  return <h1>{name}</h1>;
}
```

### 실제 소스코드 추가

#### App.jsx

```jsx
import './App.css';
import { Switch, Route, Routes } from 'react-router-dom';
import Home from './pages/Home';
import Post from './pages/Post';
import User from './pages/User/User';
import Optional from './pages/Optional';
import UserMain from './pages/User/UserMain';
import About from './pages/User/About';

function App() {
  return (
    <Routes>
      {/*
        Route에 Children이나 component 대신에, element 사용
        Route는 "Routes"의 직속 자식이어야 함
        exact는 더 이상 존재하지 않음!(v6에서는 디폴트로 생각)
      */}
      <Route path="/" element={<Home />} />
      <Route path="/posts/:id" element={<Post />} />
      <Route path="/users/:username/*" element={<User />}>
        <Route path="" exact element={<UserMain />} />
        <Route path="about" element={<About />} />
      </Route>
      {/* Optional URL 파라미터가 사라졌다.(아래와 같이 ?가 붙는)필요하면 Route를 2개 만들자. */}
      {/* <Route path="/optional/:value?" element={<Optional />} /> */}
      <Route path="/optional/:value" element={<Optional />} />
      <Route path="/optional" element={<Optional />} />
    </Routes>
  );
}
export default App;
```

#### Post.jsx

```jsx
import { useHistory, useNavigate, useParams } from 'react-router-dom';

function Post() {
  const { id } = useParams();
  /* 
    useHistory =>  useNavigate로 변경
      1. 기존 history.push = navigate('path')로 변경
   */
  const navigate = useNavigate();

  return (
    <div>
      <button
        onClick={() => {
          navigate('/');
        }}
      >
        Home
      </button>
      <button
        onClick={() => {
          // v5: history.goBack();
          navigate(-1);
        }}
      >
        Go Back
      </button>
      <button
        onClick={() => {
          // history.go(-2);
          navigate(-2);
        }}
      >
        Go Back Twice
      </button>
      <div>Post {id}</div>
      <button onClick={() => navigate(`/posts/${parseInt(id) + 1}`)}>Next Post</button>
    </div>
  );
}

export default Post;
```

### Optional.jsx

```jsx
import { useParams } from 'react-router-dom';

function Optional() {
  const { value } = useParams();
  return <div>Value: {value ?? 'None'}</div>;
}

export default Optional;
```

### pages 폴더의 User.jsx

```jsx
import { Outlet, Route, Routes, useParams, useRouteMatch } from 'react-router-dom';
import { Link } from 'react-router-dom';

function User() {
  /* useRouterMatch도 사라짐 대신에 상대 경로를 쓸 수 있게 되었음
    useRouterMatch를 쓴 이유가 match url나 match path를 읽어 "현재 경로애 *기반*하여 링크나 라우트를 설정하려고 사용했는데"
    상대경로를 이용할수 있게 되었음.
  */
  // v5: const match = useRouteMatch();

  const { username } = useParams();

  return (
    <div>
      <div>
        {/* v5: <Link to={`${match.url}`} style={{ marginRight: 16 }}> */}
        <Link to="" style={{ marginRight: 16 }}>
          @{username}
        </Link>
        {/* <Link to={`${match.url}/about`}>About</Link> */}
        {/* 주의점은 /about을 하게되면 진짜 about 페이지로 가게 된다. */}
        <Link to="about">About</Link>
      </div>
      {/* 서브 라우트를 구현하는 또 다른 방법은 Outlet이 있다. */}
      {/* 아래처럼 해도 되지만 일단 다른 케이스를 알기 Outlet을 사용하기 위해서 일단 주석 */}
      {/* <Routes>
        <Route path="" exact element={<UserMain />}></Route>
        <Route path="about" element={<About />}></Route>
      </Routes> */}
      {/* 사용하고 싶은 곳에 Outlet을 넣는다. */}
      <Outlet />
    </div>
  );
}

export default User;
```

#### 참고

[velopert님 youtube](https://www.youtube.com/watch?v=CHHXeHVK-8U&t=7s)
[React-router 공홈](https://reactrouter.com/)
[jaeme dev](https://www.jaeme.dev/react-router-v6/)
