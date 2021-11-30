---
# []는 카테고리 명 title을 적지 않으면 파일명이 올라간다.
title: "axios를 모듈화하여 사용해보자!"
#excerpt는 description "발췌부분 설정하면 이 글이 들어가고, 설정하지 않는다면 글의 첫 문단이 들어가게됨"

excerpt: "axios를 모듈화하여 사용해보자!"
# 대괄호 안에 들어가는 카테고리 : 목차
categories:
  - "react"
tags:
  - [axios]
# toc는 우측 테이블
toc: true
# 우측 테이블 내용이 따라 오는것 
toc_sticky: true
# 우측 테이블 제목
toc_label: "axios를 모듈화"

toc_icon: "cookie-bite"
# 처음 작성날짜
date: 2021-11-30
# 수정날짜
last_modified_at: 2021-11-30
---

## Axios를 프로젝트에서 사용하기

### 네트워킹을 이용할 때 먼저 src에 api 폴더를 만들어 주자

- 각 라우팅에 fetch를 사용하는 것은 비효율 적이니 src 폴더에 api.js를 만들어 주자
- axios.create()를 사용하여 **공통적으로 사용되는** 키밸류를 넣어준다.
- api.js 를 만드는 이유는 공통적으로 사용하는 코드를 줄여주기 위함

```jsx
import axios from 'axios'

const api = axios.create({
	baseURL : '해당 api url',
	//쿼리로 넘길 키들을 prams 객체에 키밸류로 순서대로 넣어준다.
	params : {
		api_key : '받은 api',
		laguage : '밸류'
	}
})

export default api
```

### 각 성질에 맞는 코드를 작성하자.

- `axios.create()` 를 사용하여 공통적으로 사용되는 부분을 정리 했으면 "각 컴포넌트 별로 맞는 공통 코드를 작성할 수 있다.
- 아래 코드는 예를 들어, url이 공통적으로 movie로 시작하거나 tv로 시작하는 url 구분으로 정리된 코드다 한번 살펴보자

```jsx
// api.get('해당 url') => 아래를 보다싶이 첫글자에 "/"가 들어가지 않는다.
//그 이유는 /을 넣으면 절대 경로가 되면서 baseUrl을 덮어쓰게 되기 때문이다.

export const moviesApi = {
  nowPlaying : () => api.get('movie/now_playing'),
  upcoming : () => api.get('movie/upcoming'),
  popular : () => api.get('movie/popular'),
  
	// 상위 3개의 코드와는 다르게 prams라는 key가 추가되었는데 이는, detail은 쿼리가 붙기 때문이다.
	movieDetail : (id) => api.get(`movie/${id}`,{
    params : {
      append_to_response : "videos"
    }
  }),

  // 첫번째 인자 url 두번째에 쿼리 prams를 넘겨준다.
  searchMovie : (terms) => api.get(`search/movie`,{
    params : {
      query : terms
    }
  })
}

export const tvApi = {
  topRated : () => api.get('tv/top_rated'),
  popular : () => api.get('tv/popular'),
  airingToday : () => api.get('tv/airing_today'),
  showDetail : id => api.get(`tv/${id}`,{
    params : {
      append_to_response : "tv"
    }
  }),

  searchTv : (terms) => api.get(`search/tv`,{
    params : {
      query : terms
    }
  })
}
```


```jsx
// 또 다른 예시
import axios from 'axios';

export const customAxios = axios.create({
  baseURL: process.env.REACT_APP_API_URL,
});

export const initRenderApi = {
  list: () => customAxios.get(`/youtube/v3/videos?part=snippet&chart=mostPopular&maxResults=25&key=${process.env.REACT_APP_API_KEY}`),
  search: text => customAxios.get(`/youtube/v3/search?part=snippet&type=video&maxResults=10&q=${text}&key=${process.env.REACT_APP_API_KEY}`),
};

// 위 코드의 search 사용에 대한 예시
const response = await initRenderApi.search(query);

```

### 해당 프로젝트에서 사용하기

- 공통 코드로 구축이 되어있는 상태다. 이제 실제 컴포넌트에서 공통 모듈로 빼 둔 해당 코드들을 불러와서 사용해 보자.

```jsx
const response = await moviesApi.nowPlaying()
```

 위 코드를 보면 moviesApi를 import를 하였고 moviesApi는 객체이며 nowPlaying은 밸류가 함수로 이뤄져 있기에 호출을 하는 형태로 되어있다.


 ### 참고
 - 노마드 코더