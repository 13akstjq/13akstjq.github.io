---
layout: post
title:  "[redux] redux개념 정리"
date:   2019-06-12 20:21:00
author: 한만섭
categories: redux
tags: react reactnative redux
---


> ### redux(State Container)란? 
우선 리덕스는 리액트만을 위한 것이 아닌 점. 리액트에서 리덕스를 사용하는 줄 알고 있었는데, 모든 곳에서 리덕스를 사용할 수 있다.  
왜 모든 곳에서 사용할 수 있는지는 `리덕스`가 어떤 친구인지 알아보면 이해가 갈것 같다.   
- 리액트는 컴포넌트들로 구성되어있는데 App컴포넌트와 하위 컴포넌트들로 구성되어있다. App에서는 `Global State`를 사용하고, 하위 컴포넌트
에서는 `local State`를 사용한다. 이렇게 전역과 지역 state를 사용해야하기 때문에 우리는 redux를 필요로 한다.  
- 전역과 지역 state를 사용하기 때문에 state를 공유해야하는 상황이 필연적으로 발생할텐데 그것을 쉽게 해결하기 위해 `redux`를 사용한다.  
- 공유해야하는 `Global State`를 특정 장소에 저장해놓고 쉽게 불러 쓴다면 어떨까? 라는 생각에서 시작한것 같다.  

그래서 redux를 `State Container`라고 부르는 것이다.  
***
> ### redux를 언제 사용해야하는가? 
리덕스를 사용하지 않을 경우를 말해보려한다. 블로그 게시물의 댓글을 달 때 로그인 정보가 필요한데 댓글에서 필요한 로그인 정보를 위해 게시물 컴포넌트를
댓글 컴포넌트에게 로그인정보를 전달하는 것은 `useless state`가 발생하기 때문에 좋지 않다. 혹은, navbar에서 "akstjq님 환영합니다."라는 문구를 띄우고
 싶다면 navbar 컴포넌트에게도 로그인정보를 넘겨주어야하는가? 아닌 것이다.  
 
 이럴 때 로그인 정보를 담고 있는 `State Container`가 있다면 거기서 꺼내서 사용하면 되는 것이다.  
 이렇게 하위 컴포넌트에게 state를 전달하기 위해 불필요한 과정을 생략하기 위해 Shared Stated를 Container에 묶어놓는 것 이것을 redux라고 하는 것이다.    
 ***
 > ### redux는 어떻게 사용하는가?  
 redux는 State를 담고있는 Store이다. 객체(object)를 관리하고 있기 때문에 굉장히 엄격하다. 그래서 직접적으로 수정이 불가능하다. `reducer`라는 중간점
 이 있다. 우리는 reducer에게 `action`을 보내게 되고 그 `action`을 받아 `reducer`가 store안의 object를 수정해주는 것이다.  
 
 실제 사용하는 방법에 대해서는 다음에 또 글을 올리겠다.  
