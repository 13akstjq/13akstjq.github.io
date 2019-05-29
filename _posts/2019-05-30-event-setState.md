---
layout: post
title:  "[React] event setState 및 bind 개념  "
date:   2019-05-29 21:27:59
author: 한만섭
categories: react
tags: React Component module
---


## .bind()

컴포넌트 내에서 이벤트를 호출할 일은 많을 것이다. 그 이벤트를 통해서 컴포넌트의 state를 변경하고 싶은 상황도 많을 것이다.  
그때 이벤트 함수는 컴포넌트의 state에 어떻게 접근해야하는가?  
this로 접근할 수 있다고 생각했지만 아니였다. this는 함수 내의 선언되지 않은 this를 의미했다.  
그래서 함수가 컴포넌트를 `this`라고 인식할 수 있게 해주어야했다. 
```
<a href="/" onClick={function(e){
                console.log(e);
                e.preventDefault();
                // this.state.mode = 'welcome';
                this.setState({
                  mode:'welcome'
                });
                
              }.bind(this)}>{this.state.subject.title}</a>
```
`<a>tag`를 클릭했을 때 이벤트를 주게 되면 위와 같이 함수가 끝나는 시점에 `.bind(this)`를 해준다.  
이 의미는 이 함수에게 App 컴포넌트자체를 파라메터로 넘겨주겠다는 의미다. 즉, 이벤트 함수는 컴포넌트객체를 인자로 받게 되고,
받았기 때문에 this로 사용할 수 있다는 뜻이다.   


추가로 react는 유사 JavaScript이기 때문에 문법이 모두 일치하지 않는다.
js -> onclick()
react -> onClick={function(){}}



## setState

컴포넌트에는 render하기전에 호출하는 `constructor`가 있는데 그 안에 `state`라는 component의 변수? 를 담는 것이 있다.  
동적인 페이지를 만들기 위해서 이벤트를 사용할텐데 이벤트를 사용해서 컴포넌트의 state를 변경하려고 할때 조심해야한다.  

일반적으로 변수같기 때문에 그냥 담으려고 한다.
```java
this.state.mode = 'welcome';
```
하지만 위의 코드는 리액트가 알지 못한다. `constructor`를 만들 때는 생성자 개념이라 바로 state값을 초기화하는 것이 가능하지만,  
이벤트를 통해서 변경시킬경우 설정자를 통해서 변경해야한다.  
```java
 this.setState({
                  mode:'welcome'
                });
```
