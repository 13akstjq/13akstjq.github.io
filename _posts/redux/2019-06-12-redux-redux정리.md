---
layout: post
title:  "[redux] redux개념 정리"
date:   2019-06-12 20:21:00
author: 한만섭
categories: redux
tags: react reactnative redux
---

* TOC
{:toc}


> ### redux(State Container)란? 
우선 리덕스는 리액트만을 위한 것이 아닌 점. 리액트에서 리덕스를 사용하는 줄 알고 있었는데, 모든 곳에서 리덕스를 사용할 수 있다.  
왜 모든 곳에서 사용할 수 있는지는 `리덕스`가 어떤 친구인지 알아보면 이해가 갈것 같다.   
- 리액트는 컴포넌트들로 구성되어있는데 App컴포넌트와 하위 컴포넌트들로 구성되어있다. App에서는 `Global State`를 사용하고, 하위 컴포넌트
에서는 `local State`를 사용한다. 이렇게 전역과 지역 state를 사용해야하기 때문에 우리는 redux를 필요로 한다.  
- 전역과 지역 state를 사용하기 때문에 state를 공유해야하는 상황이 필연적으로 발생할텐데 그것을 쉽게 해결하기 위해 `redux`를 사용한다.  
- 공유해야하는 `Global State`를 특정 장소에 저장해놓고 쉽게 불러 쓴다면 어떨까? 라는 생각에서 시작한것 같다.  
redux를 쉽게 표현하자면 `Shared State를 저장하는 Store`라고 표현할 수 있을 것같다. 그러면 `Shared State`에 대해서 일단 얘기해보려고한다.  

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
 

> ### Shared State
컴포넌트로 이루어진 리액트 내에서 State는 중요한 역할을 가진다. 각 컴포넌트의 **상태**를 담아주는데, 필요한 경우에 여러 컴포넌트에서 하나의 
State에 접근해야 하는 상황이 생길 수 있다. 그렇다면 그럴 때마다 root 컴포넌트애서 leaf 컴포넌트로 props로 전달해주어야 하는 것인가? **당연히 아니다.**  
이렇게 Shared State를 사용하기위해 `Store`라는 것을 만들어낸 것이다.  


> ### Store  
Shared State를 저장해놓고 불러서 쓸 수 있는 저장소 역할을 해주는 것이 `Store`이다. Store안에 `Redux`가 있는 것이라고 생각하면 된다.  


> ### Connect
자주 사용하는 State를 Store에 저장해놓았다면 컴포넌트들은 Store에 접근해 원하는 State를 사용할 수 있을 것이다. 근데 공유되는 State는 **매우 중요하다. **
그렇기 때문에 쉽게 접근해서는 안된다. 그래서 나온 것이 `Dispatch`를 이용해 `action`을 Store에게 넘겨주는 것이다. 그 과정에서 필요한 것이 Store와 
컴포넌트를 연결(Connect)해주는 것이다. Store와 컴포넌트를 연결했다면 이제 드디어 Props를 전달전달해서 하위 컴포넌트에서 사용하는 것이 아니라, 
`Store`라는 저장소에서 꺼내 쓸 수 있는 환경이 만들어지는 것이다.  
　  

***

　  

> ## 정리  
1. 자주 사용해야하는 `Global State`를 `Redux`형태로 `Store`에 저장해놓는다.  
2. 그래서 필요한 컴포넌트는 Store와 `connect`를 통해 연결한다.  
3. 연결했다면 `Dispatch`를 통해 `action`을 Store에게 전달하여 사용한다. 


 
 
