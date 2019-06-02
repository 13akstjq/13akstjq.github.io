---
layout: post
title:  "[React] react에서 ajax사용하기"
date:   2015-04-18 08:43:59
author: 한만섭
categories: react
tags:	react ajax
---

> ## 시작하기 전에...

매일 jquery에서 사용하던 ajax를 이제는 react에서 사용해볼 수 있다는게 너무 기쁘다.  
Ajax(Asynchronous JavaScript and XML, 에이잭스)는 비동기로 xml 파일을 가져올 수 있는 매우 좋은 기술이다.  하지만, 최근엔 xml 파일을 위한 것이 아니라 
json 파일을 위해 사용되고 있기 때문에 `ajaj`라고 부르는 개발자들도 있다고 한다.  

> ### react의 ajax
jquery 에서 사용하던 ajax 보다 굉장히 심플하다.  
아직 잘 모르지만 `fetch`를 이용해서 하면 매우 심플하게 작성할 수 있다고 한다.  

> ### api이용해보기 

ajax를 사용해보기 위해서는 api서버가 있어야 하는데, 영화 페이지를 제작하고 있었기 때문에 영화 정보를 제공하는 [YTS](https://yts.am/api)를 이용해보려고 한다. 


빨간 네모 부분을 사용하면 된다. 
![image](https://user-images.githubusercontent.com/46010705/58760536-1fef7c80-8574-11e9-8dbe-fc6b22fae704.png)


fetch안에 넣어서 사용하면 간단히 사용가능하다.  
```
componentDidMount( ){
   console.log(fetch('https://yts.am/api/v2/list_movies.json?'))
  }
```  



그리고 아래와 같이 _renderMovies라는 함수를 만들어 App의 길이를 줄여줄 수 있다.  
사용자 함수는 `_renderMovies`와 같이 _를 붙히는 것이 일반적이다.  
또한 현재 `state`의 상태를 확인하는 3항연산자를 사용하여 loading화면을 만들 수 있다.  

component code  
```javascript
class App extends Component {
 
state = {}

  componentDidMount( ){
   console.log(fetch('https://yts.am/api/v2/list_movies.json?'))
  }

  _renderMovies = () => {
    this.state.movies.map((movie, index) =>{
      return <Movie title={movie.title} poster={movie.poster} key={index}/>
    })
  }
  render(){
    console.log('render');
    return (
      <div className="App">
        {this.state.movies ? this._renderMovies : 'Loading'}
      </div>
    );
  }
}
```
