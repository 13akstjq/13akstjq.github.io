---
layout: post
title:  "[React] 프로젝트 구성 확인하기"
date:   2019-05-29 19:12:59
author: 한만섭
categories: React
tags: React
---

* TOC
{:toc}



## react 프로젝트의 구성 알아보기 

`public`에 있는 `index.html`이 가장 먼저 켜지는 놈인것 같음.  

`src`에 있는 것이 component가 되는 것 같다. 

`index.js`에 있는 
```
ReactDOM.render(<App />, document.getElementById('root'));
``` 
로 인해서 `public` 에 있는 `index.html`의 `id=root`가 결정된 것이다.  
```
import App from './App';
```
App이라는 component? 를 import 해서 사용하는 것 같다. 


### react component는 function 과 객체 형식 존재 
react는 function 형식과 객체 형식으로 component를 작성할 수 있는데, 강의에서는 객체 형식으로 사용함. 

```
import React, { Component } from 'react';

class App extends Component {
  render(){
    return (
      <div className="App">
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <p>
            Edit <code>src/App.js</code> and save to reload.
          </p>
          <a
            className="App-link"
            href="https://reactjs.org"
            target="_blank"
            rel="noopener noreferrer"
          >
            Learn React
          </a>
        </header>
      </div>
    );
  }
}
```

위의 형식으로 작성하면 됨.  


`index.js`
```
ReactDOM.render(<App />, document.getElementById('root'));
```
`<App />`는 사용자 정의 태그로서 위의 클래스이다. 


