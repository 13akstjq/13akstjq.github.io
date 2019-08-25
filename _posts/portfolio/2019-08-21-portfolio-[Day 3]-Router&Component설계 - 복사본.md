---
layout: post
title:  "[Portfolio][Day 3] Router & Component 설계"
date:   2019-08-21-20:13:00
author: 한만섭
categories: portfolio
tags: portfolio
---









[TOC]





# 프로젝트 설계

파이어 베이스의 설정을 마쳤기 때문에 **Router **및 **Component **설계를 통해서 프로젝트의 큰 틀을 잡으려고 합니다. 우선 Router부터 하겠습니다.  



## 1. Router 



Router는 크게 **Home** , **PostDetail**, **Blog**, **Conference**로 나누려고 합니다. 로그인을 redirect로 하는 것이 아니기 때문에 로그인을 하는데 Router가 필요하지는 않을 것 같습니다.  

`Router.js`

```js
import React from "react";
import { HashRouter as Router, Switch, Route } from "react-router-dom";
import Detail from "./Views/Detail";
import Home from "./Views/Home";

export default () => {
  return (
    <Router>
      <Switch>
        <Route path="/" exact component={Home} />
        <Route path="/post/:category/:id" component={Detail} />
      </Switch>
    </Router>
  );
};
```



## 2. Components

컴포넌트 설계가 중요하다는 것은 공부를 하면서 프로젝트를  하면서 많이 느꼈던 점이기 때문에 제대로 해보고 넘어가보려고 합니다.  각 **Router**에 어떤 컴포넌트가 들어갈지 생각하며 설계해보도록 하겠습니다.  

### - Main 

​	Main페이지에 필요한 것은 크게 **Header** , **SideMenu** , **ProjectCard** 인 것 같습니다.  

- #### Header 

​		Logo

​		Login

- #### SideMenu

- #### ProjectCard



​	

### - PostDetail



### - Blog





### - Conference





