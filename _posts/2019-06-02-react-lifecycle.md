---
layout: post
title:  "[react] react의 lifecycle "
date:   2019-06-02
author: 한만섭
categories: react
tags: react lifecycle
---

## 1. render에서의  lifecycle의 의미 

모든 컴퓨터 언어에서 생성되고 소멸되는 과정이 존저하듯이 `component`에게도 생성되기까지의 과정이 존재한다.  
생성부터 소멸까지의 과정을 익히고 있으면 오류를 찾아내거나 프로그래밍을 하는데 많은 도움을 줄 수 있을 것이다. 

### react component의 render lifecycle

1. componentWillMount()  
component가 아직 생성되기 전에 호출되는 함수이다.  

2. render()
컴포넌트가 생성되면서 여러 tag를 그릴 때 호출되는 함수이다. 

3. componentDidMount()  
component가 생성된 후에 호출되는 함수이다. 



### render lifecycle 생성주기 확인해보기 

아래와 같이 component내부에 함수를 선언해놓고 console로 확인해보면 
```javascript
class App extends Component {
  componentWillMount( ){
    console.log('will mount');
  }
  
  componentDidMount( ){
    console.log('did mount');
  }
  render(){
    console.log('render');
    return (
      <div className="App">
        {movies.map((movie, index) =>{
          return <Movie title={movie.title} poster={movie.poster} key={index}/>
        })}
      </div>
    );
  }
}
```  


이런 결과를 얻을 수 있다. 
```console
App.js:25 will mount
App.js:32 render
App.js:29 did mount
```



## 2. update 주기 알아보기 

1. componentWillReceiveProps()  
2. shouldComponentUpdate()  ==> old prop과 달라진 것이 있다면 update를 함 
3. componentWillUpdate()  ==> 로딩 스피너 같은 것을 넣을 수 있다. 
4. render()  
5. componentDidUpdate()

