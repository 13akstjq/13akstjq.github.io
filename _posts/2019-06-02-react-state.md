---
layout: post
title:  "[react] react에서 state사용해보기 "
date:   2019-06-02 
author: 한만섭
categories: react
tags: react state
---


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

