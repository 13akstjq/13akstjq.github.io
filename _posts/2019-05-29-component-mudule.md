---
layout: post
title:  "[React] React Component 모듈화 하기 "
date:   2019-05-29 20:49:59
author: 한만섭
categories: react
tags: React Component module
---

# 컴포넌트 모듈화 하기 

지금까지는 컴포넌트를 `App.js`에 모두 넣어 놓았는데, 객체지향에 맞지 않는 방식이라고 생각했다.  
그런 생각을 할때 쯤 바로 컴포넌트를 모듈화 하는 방법을 배웠다.  
컴포넌트는 하나의 `클래스`라고 생각하면 되기 때문에 `클래스`를 각각의 파일로 나누는 것이다. 


- Content 컴포넌트를 모듈하기  
  react가 필요하기 때문에 import 해주고  
  이 파일을 사용할 수 있도록 export 해주는 것 말고는 특별한것은 없다. 
  
```
import React ,{Component} from "react";

class Content extends Component{
    render(){
      return(
        <article>
        <h2>{this.props.title}</h2>
        {this.props.desc}
    </article>
      );
      }
  }
  export default Content;
```
  
  - App.js에서 import 해서 사용하기  
    놓여진 파일 경로만 맞춰주면 import해서 사용할 수 있다. 
 ```
  import TOC from "./components/TOC";
  import Subject from "./components/Subject";
  import Content from "./components/Content";
```
  
  
