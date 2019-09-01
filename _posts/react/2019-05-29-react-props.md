---
layout: post
title:  "[React] props 사용해보기 "
date:   2019-05-29 20:49:59
author: 한만섭
categories: react
tags: React props 
---

* TOC
{:toc}

## props 사용해보기 

예전에 잠깐 vue를 사용해봤을 때 `vue`와 `react`가 비슷하다는 얘기를 들었다. 이번에 props를 간단히 써보면서 다시한번 느꼈는데  
진짜 언어는 크게 변하는 것은 없는 것 같다. 

vue의 {{}}가 --> react에서는 {}로 바뀐 것 같다. 


subject라는 component를 사용할 때 항상 똑같은 component를 사용한다면 그 가치가 떨어질 것이다.  
물론 똑똑하신 분들이 이미 그 불편함을 해소해주었다.  

사용자 정의 tag인 `Subject`에 tag `props`를 정의 해놓을 수 있다.
즉, 데이터가 들어가야할 구멍들을 의미한다. ( 추후에, 통신해서 받아온 데이터를 끼워 넣는 곳이 될것 같다.)  

title과 sub라는 props 정의하기
```javascript
<Subject title="WEB" sub="world wide web!!"></Subject>
```

정의를 했다면 끼워 넣을 공간을 할당해야하지 않을까?  
```javascript
class Subject extends Component{
  render(){
    return(
      <header>
            <h1>{this.props.title}</h1>
            {this.props.sub}
        </header>
    );
  }
}
```

이제 `Subject`컴포넌트는 title , sub라는 props를 가지고 있는 객체가 되는 것이다. (내 이해 방법) 
그 객체 안의 변수에 값을 넣어주는 곳이 `App`tag인 것이다. tag를 선언할 때 `props`에 그 컴포넌트를 사용하는 props를 써주면 된다.  
이렇게 할 경우 같은 컴포넌트를 사용한다고 하더라도, 다양한 모습으로 사용할 수 있다. 
