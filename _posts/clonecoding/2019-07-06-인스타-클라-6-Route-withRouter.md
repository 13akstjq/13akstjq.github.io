---
layout: post
title:  "[인스타클론코딩] [FrontEnd].6 - react-router-dom정리 "
date:   2019-07-06-20:09:00
author: 한만섭
categories: clonecoding
tags: react instagram react-router-dom
---

> ## To Do
- [] react-router-dom 정리
- [] Router, Link, Route 정리
- [] withRouter 정리 
- [] switch 
　  

***

　  
> ### react-router-dom

react-router-dom은 [npm](https://www.npmjs.com/package/react-router-dom)으로 설치할 수 있습니다.  

> ### router란?  

특정 주소로 유저가 접근했을때, 그 URL에 맞는 작업을 Client Side에서 할 수 있도록 해주는 라이브러리입니다. 공식적으로 제공하는 것은 아니지만 가장 많이 사용하고 있습니다.  


> ### hashRouter  
react-router-dom은 단일 페이지 앱의 내비게이션 이력을 관리할 수 있는 다양한 방법을 제공한다. HashRouter는 클라이언트를 위해 설계된 라우터다. Hash(#)는 앵커링크를 정의할 때 쓰였다. 주소창의 현재 페이지 경로 뒤에 # 식별자를 입력하면 브라우저는 서버 요청을 보내지 않고, 현재 페이지에서 식별자에 해당하는 id 애트리뷰트가 있는 엘리먼트를 찾아서 그 엘리먼트의 위치를 화면에 보여준다. 

사용하는 방법은 간단합니다.  

```javascript
import { HashRouter as Router } from "react-router-dom";
```

> ### Link  

Link는 해당 경로로 이동시켜주는 기능을 합니다. 이 것은 **a**와 같은 기능을 한다고 생각할 수 있는데요. 비슷하긴 하지만 새로고침을 하지 않는 다는 성능상의 
이점이 있습니다.  

Link를 사용할 때는 **Router**안에 선언해서 사용해야 합니다. Link의 사용방법에 대해서 알아보도록 하겠습니다. 대단한거 없이 아래처럼 사용하면 됩니다.  

```javascript
<Link to="/explore">
            <Compass />
          </Link>
```

> ### Route  

- path 속성으로 경로를 지정합니다.
- render, component, children 으로 렌더링을 실시합니다.  
- 실제 경로와 완벽하게 일치 하지 않더라도 포함되는 경우에 렌더링을 실행합니다.  
 - 정확히 매칭될때만 렌더링을 하고싶다면, exact 속성을 true로 지정해주세요.  
 - 컴포넌트에 match, history, location 이라는 객체를 넘겨줍니다.
 
사용 방법은 아래와 같습니다. 홈경로와 같이 **/**는 exat로 정확하게 입력되어야합니다. 여러 Route를 Router가 가져야할 때는 **Switch**를 사용합니다.  


```javascript
const LogedInRouter = () => (
  <Switch>
    <Route exact path="/" component={Feed} />
    <Route path="/explore" component={Explore} />
    <Route path="/notifications" component={Notifications} />
    <Route path="/search" component={Search} />
    <Route path="/:username" component={Profile} />
  </Switch>
);
```
