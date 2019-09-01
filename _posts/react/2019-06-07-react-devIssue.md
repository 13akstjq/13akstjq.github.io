---
layout: post
title:  "[React] 리액트를 개발 이슈 "
date:   2019-06-07 01:30:00
author: 한만섭
categories: react
tags: react tip 
---

* TOC
{:toc}



> ### return의 생략 경우 
```
const add = ( a, b ) => ( a + b )
```
```
const add = ( a, b ) => { return a+b; }
```
두 코드는 동일하다. => 뒤에 ()를 쓸때는 자동으로 return이 내장되어있기 때문이다.

> ### prevState 사용해야하는 경우 

```
state = {
   count: 0
}
updateCount = () => {
    this.setState({ count: this.state.count + 1});
    this.setState({ count: this.state.count + 1});
    this.setState({ count: this.state.count + 1});
    this.setState({ count: this.state.count + 1});
}
```
위 코드를 실행하면 count는 4가 아닌 1이 나온다. 이것을 해결하기 위해 prevstate를 사용한다. 

```
updateCount = () => {
    this.setState(prevstate => ({ count: prevstate.count + 1}));
    this.setState(prevstate => ({ count: prevstate.count + 1}));
    this.setState(prevstate => ({ count: prevstate.count + 1}));
    this.setState(prevstate => ({ count: prevstate.count + 1}));
}
```
위처럼 사용하면 count에 4를 담을 수 있다. 


> ### React를 import하지 않을 경우 발생하는 에러 

  ![image](https://user-images.githubusercontent.com/46010705/60530520-e311d380-9d33-11e9-894d-fe41a3573aa5.png)
  이 코드를 넣으면 해결됩니다.  
  ```
  import React from "react";
  ```
