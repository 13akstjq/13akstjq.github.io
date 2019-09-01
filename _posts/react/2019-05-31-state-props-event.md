---
layout: post
title:  "[React] state와 props 비교  "
date:   2019-05-31 20:00:59
author: 한만섭
categories: react
tags: react state props
---

* TOC
{:toc}


> **state**와 **props**의 비교 
간단하게 설명하자면  
`props`는 부모 컴포넌트가 자식 컴포넌트에게 주는 값이다. 자식 컴포넌트는 props를 수정할 수 없다. `readonly`  
`state`는 컴포넌트 내에서 선언하는 것이며 값을 변경할 수 있다. 

### -하위 컴포넌트에게 값 전달하기
react프로그래밍 하다보면 하위 컴포넌트에게 값을 주어야할 때가 있다. 그럴때 하위 컴포넌트의 태그 안에 `props`를 선언할 수 있다.  
즉, 상위 컴포넌트가 주는 값이라는 뜻이다. 그 값은 수정 불가능하며 수정하게 되면 `readonly` error를 만나게 될 것이다. 

### -상위 컴포넌트의 state값 변경하기 
상위컴포넌트만 하위컴포넌트에 관여할 수 있는가? 대답은 아니다. 하위 컴포넌트에서 직접적으로 상위컴포넌트의 `state`에 접근할 수는 없다.  
그렇다면 어떤 방법으로 상위컴포넌트의 `state`에 접근한다는 것인가?  

위에서 말한 `props`는 문자,숫자같은 기본형 뿐만 아니라 function(){}까지도 `props`로 사용할 수 있다. 즉, 이것을 이용하는 것이다.  
하위컴포넌트에서 함수를 호출하며 인자로 여러 파라메터들을 넘겨준다.  
 
 #### e객체 활용하기 
 파라메터로 넘겨주기전에 어떤 값을 넘겨줄지 개발자 도구의 e객체 안을 살펴보면 된다. 
 -input의 경우 e객체 안에 아래와 같은 형태로 들어있다.  
 ``` 
 e.target.(input의 name).value
 ```
 이렇게 하위 컴포넌트에서 보낼 값은 e를 통해서 넘기는 것 같다.~~(뇌피셜)~~
 
 나중에는 `redus`?라는 것으로 관리한다고 하는데 아직은 이해하지 못했다. 
 
 
 ## .bind()

컴포넌트 내에서 이벤트를 호출할 일은 많을 것이다. 그 이벤트를 통해서 컴포넌트의 state를 변경하고 싶은 상황도 많을 것이다.  
그때 이벤트 함수는 컴포넌트의 state에 어떻게 접근해야하는가?  
this로 접근할 수 있다고 생각했지만 아니였다. this는 함수 내의 선언되지 않은 this를 의미했다.  
그래서 함수가 컴포넌트를 `this`라고 인식할 수 있게 해주어야했다. 
```javascript
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
```javascript
js -> onclick()
react -> onClick={function(){}}
```


## setState

컴포넌트에는 render하기전에 호출하는 `constructor`가 있는데 그 안에 `state`라는 component의 변수? 를 담는 것이 있다.  
동적인 페이지를 만들기 위해서 이벤트를 사용할텐데 이벤트를 사용해서 컴포넌트의 state를 변경하려고 할때 조심해야한다.  

일반적으로 변수같기 때문에 그냥 담으려고 한다.
```javascript
this.state.mode = 'welcome';
```
하지만 위의 코드는 리액트가 알지 못한다. `constructor`를 만들 때는 생성자 개념이라 바로 state값을 초기화하는 것이 가능하지만,  
이벤트를 통해서 변경시킬경우 설정자를 통해서 변경해야한다.  
```java
 this.setState({
                  mode:'welcome'
                });
```
 
 
 ## react에서 state 이해하기 

가장 중요한 것은 `state`가 변경될 경우 `render`가 호출되는 것이다.



### state 사용해보기 

state를 사용할 때는 key : value 형식으로 사용한다. 
```javascript
state = {
    movies : [
      {
        title:"matrix",
        poster:  'https://images-na.ssl-images-amazon.com/images/I/51EG732BV3L._SY445_.jpg'
      },
      {
        title:"full Metal jacket",
        poster:   'https://i.pinimg.com/736x/36/1e/cd/361ecdb85a3767f70810cbe2cdaaf1a4.jpg'
      },{
        title:"oldboy",
        poster:  'https://upload.wikimedia.org/wikipedia/ko/thumb/4/48/Old_Boy.jpg/250px-Old_Boy.jpg'
      },{
        title:"star wars",
        poster:  'https://starwarsblog.starwars.com/wp-content/uploads/2015/10/tfa_poster_wide_header-1536x864-959818851016.jpg'
      }
    ]
  }
```

### state update 해보기 

state를 수정할 때는 setState를 사용해야한다. 직접적으로 변경하게 될 경우 React가 인식하지 못하여서 `render`를 해주지 않는다.  
```...this.state.movies```는 이 컴포넌트의 state중에 movies의 값을 의미한다.  
따라서, 기존의 값 + 새로 추가된 값을 `movies`라는 state에 넣기 위함이다.  
사실, 배열이기 때문에 `push`를 해주면 된다고 생각했는데 찾아봐야겠다. 
```
this.setState({
        movies : [
          // ...this.state.movies,
          {
            title: "transporting",
            poster:'https://starwarsblog.starwars.com/wp-content/uploads/2015/10/tfa_poster_wide_header-1536x864-959818851016.jpg'
          }
        ]
      })
```      
 
 
