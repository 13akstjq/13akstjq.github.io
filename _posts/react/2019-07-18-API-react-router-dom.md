---
layout: post
title:  "[React] react-router-dom 정리"
date:   2019-07-18 17:46:00
author: 한만섭
categories: react
tags: react router
---





[공식사이트](https://reacttraining.com/react-router/web/api/Redirect)





`switch`가 없을 경우 `./tv/popular`링크에 들어가면   

`Router.js`

```js
export default () => (
  <Router>
      <Route path="/" exact component={Home} />
      <Route path="/tv"  component={TV} />
      <Route path="/tv/popular"  component={() => <div>popular</div>} />
      <Route path="/search" component={Search} />
      <Redirect from="*" to="/" />
  </Router>
);
```

![1563380910189](C:\Users\mshan\AppData\Roaming\Typora\typora-user-images\1563380910189.png)

위 데이터가 나온다 .  



`switch`가 있을 경우  `./tv/popular`링크에 들어가면   

`Router.js`

```js
export default () => (
  <Router>
    <Switch>
    <Route path="/" exact component={Home} />
    <Route path="/tv" component={TV} />
    <Route path="/tv/popular" component={() => <div>popular</div>} />
    <Route path="/search" component={Search} />
    <Redirect from="*" to="/" />
    </Switch>
  </Router>
);
```

![1563380981780](C:\Users\mshan\AppData\Roaming\Typora\typora-user-images\1563380981780.png)

Tv만 나오는 이유는 `switch`가 여러개의 router중에서 하나만 고르기 때문이다.  

그렇다면 popular를 나오게 하고 싶다면 어떻게 해야할까.  

정확한 링크일 때만 보여주는 `exact`를 사용하면 된다.  

`Router.js`

```js
export default () => (
  <Router>
    <Switch>
      <Route path="/" exact component={Home} />
      <Route path="/tv" exact component={TV} />
      <Route path="/tv/popular" component={() => <div>popular</div>} />
      <Route path="/search" component={Search} />
      <Redirect from="*" to="/" />
    </Switch>
  </Router>
);
```



![1563381066459](C:\Users\mshan\AppData\Roaming\Typora\typora-user-images\1563381066459.png)

Route는 위에서 부터 아래로 먼저 맞는 경로가 있을 경우에 보여주기 때문에 

`Router`내부에 있는 `Route`의 순서를 신경써서 작성해야겠다.  



> ### HashRouter 

사이트에 `#`가 붙는 방식의 Router



> ### BrowserRouter  

기존 사이트처럼 `/`만 붙는 방식의 Router  



> #### Redirect  

지정한 페이지로 보내버리는 역할을 한다. 

`Router.js`

```js
<Redirect from="*" to="/" />
```



> ### Link 

`a`와는 다르게 새로고침을 하지 않는 방식의 페이지 이동 방법.  



