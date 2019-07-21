---
layout: post
title:  "[React] 영화 웹 만들면서 있었던 이슈정리 "
date:   2019-07-21 18:47:00
author: 한만섭
categories: react
tags: react 
---

> #### tail router 방법 

`search/121`같은 경로로 이동하기 위해 `/search`에 `exact`속성을 넣어주었음. 

```js
<Route path="/search" exact component={Search} />
<Route path="/search/:id" component={Detail} />
```



> #### Container 안에서 무한루프에 빠짐  

아래와 같이 코드를 작성하면 무한루프에 빠짐  

이유 : `useState`의 `set`메소드가 `render`를 다시하기 때문에 계속 `getData`함수가 호출 됨.  

```js
const getData = async () => {
    try {
      const { data: showDetail } = await tv.showDetail(121);
      const { data: movieDetail } = await movie.movieDetail(121);
      setShowDetail(showDetail); // set을 하게되면 다시 render가 됨 
      setMovieDetail(movieDetail);
    } catch (e) {
      setError(e);
    } finally {
      setLoading(false);
    }
  };

getData();
```





hooks`를 사용해서 코드를 작성할 때는 위와 같이 한번만 호출하도록 작성해야함. 

```js
const getData = async () => {
    try {
      const { data: showDetail } = await tv.showDetail(121);
      const { data: movieDetail } = await movie.movieDetail(121);
      setShowDetail(showDetail);
      setMovieDetail(movieDetail);
    } catch (e) {
      setError(e);
    } finally {
      setLoading(false);
    }
  };
// hooks의 didcomponent와 같은 기능을 하는 useEffect사용
  useEffect(() => {  
    getData();
  }, []);
```

`hooks`를 사용해서 코드를 작성할 때는 위와 같이 한번만 호출하도록 작성해야함. 





> ### 함수 사용시 주의 사항 

```js
  const getData = async (isMovie, term) => { // {isMovie,term}이라고 쓰면 안됨. 객체가 아니기 때문?
    console.log(isMovie, term);
    try {
      if (isMovie) {
        const { data: detail } = await movie.movieDetail(term);
        setDetail(detail);
      } else {
        const { data: detail } = await tv.showDetail(term);
        setDetail(detail);
      }
    } catch (e) {
      setError(e);
    } finally {
      setLoading(false);
    }
  };

  useEffect(() => {
    getData(isMovie, term);
  }, []);
```





> #### grid wrap 

영화 카드를 grid 형태로 나열하는 과정에서 화면이 작아지면 떨어지게 해야하는 방법

```js
  grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));

```



> #### animation 

styled-components에서 keyframe을 사용한 animation 

```js
const spin = keyframes`
  from {
   transform  : rotate(0deg);
  }to {
   transform  : rotate(360deg);
  }
`;
const Wrapper = styled.div`
  width: 100%;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  animation: ${spin} 3s infinite;
`;

```





> #### calc

```js
height: calc(100vh - 50px);
```





> #### react helmet 으로 site header 표시하기 

* npm 설치하기 

  ```bash
  npm add react-helmet
  ```

* import 추가하기 

  ```js
  import {Helmet} from 'react-helmet';
  ```

* 사용하기 

  ```
  <Helmet>
          <title>Search | MANFLEX</title>
  </Helmet>
  ```

