---
layout: post
title:  "[redux] redux 완벽 정리하기 "
date:   2019-08-30 18:41:00
author: 한만섭
categories: redux
tags: react redux action actioncreator dispatch connect mapstatetoprops mapdispatchtoprops 
---



[TOC]





## 서론


> ### redux(State Container)란? 
>
> 우선 리덕스는 리액트만을 위한 것이 아닌 점. 리액트에서 리덕스를 사용하는 줄 알고 있었는데, 모든 곳에서 리덕스를 사용할 수 있습니다.  
> 왜 모든 곳에서 사용할 수 있는지는 `리덕스`가 어떤 친구인지 알아보면 이해가 갈것 같습니다.  
- 리액트는 컴포넌트들로 구성되어있는데 App컴포넌트와 하위 컴포넌트들로 구성되어있다. App에서는 `Global State`를 사용하고, 하위 컴포넌트
에서는 `local State`를 사용한다. 이렇게 전역과 지역 state를 사용해야하기 때문에 우리는 redux를 필요로 한다.  
- 전역과 지역 state를 사용하기 때문에 state를 공유해야하는 상황이 필연적으로 발생할텐데 그것을 쉽게 해결하기 위해 `redux`를 사용한다.  
- 공유해야하는 `Global State`를 특정 장소에 저장해놓고 쉽게 불러 쓴다면 어떨까? 라는 생각에서 시작했습니다.  
redux를 쉽게 표현하자면 `Shared State를 저장하는 Store`라고 표현할 수 있을 것같다. 그러면 `Shared State`에 대해서 일단 얘기 해보려고 합니다.  

그래서 redux를 `State Container`라고 부르는 것이다.  

***

> ### redux를 언제 사용해야하는가? 
리덕스를 사용하지 않을 경우를 말해보려한다. 블로그 게시물의 댓글을 달 때 로그인 정보가 필요한데 댓글에서 필요한 로그인 정보를 위해 게시물 컴포넌트를
댓글 컴포넌트에게 로그인정보를 전달하는 것은 `useless state`가 발생하기 때문에 좋지 않다. 혹은, navbar에서 "akstjq님 환영합니다."라는 문구를 띄우고
 싶다면 navbar 컴포넌트에게도 로그인정보를 넘겨주어야하는가? 아닌 것이다.  

 이럴 때 로그인 정보를 담고 있는 `State Container`가 있다면 거기서 꺼내서 사용하면 되는 것이다.  
 이렇게 하위 컴포넌트에게 state를 전달하기 위해 불필요한 과정을 생략하기 위해 Shared Stated를 Container에 묶어놓는 것 이것을 redux라고 하는 것이다.     

### 

***



## 본론 

## 1. redux 

 위에서 redux에 대해서 간단히 알아봤다면 이제 redux를 파헤쳐보려고 합니다. 우선 redux에는 어떤 것들이 필요한지 알아보도록 하겠습니다.  

- **Action**
- **Action Creater**
- **Dispatcher**
- **Reducer**
- **Store**

위의 다섯개 항목은 redux의 필수 개념이라고 보시면 됩니다. 그럼 하나씩 정리해보도록 하겠습니다.  

***



### 1-1. Action

action은 중앙 저장소에 저장된 state에  **"무슨"** 동작을 할 것이지 적어놓는 **객체**입니다. action에는 `type`이 필수로 필요합니다. type은 공식문서에 나와있는 방법으로 작성하는 것을 권장합니다.  

```js
//action types
const ADD_TODO = 'ADD_TODO'
```

중앙 저장소에 저장된 state를 변경하기 위해서는 "add, update, delete"와 같은 동작의 **type**이 필요하고 "add" 의 경우에는 어떤 것을 추가해줄지도 파라메터도 필요하기 때문에 Action을 작성하면 아래와 같이 작성할 수 있습니다.  `text`는 제가 넘겨주는 데이터입니다. 무조건 text가 아닙니다.  

```js
{
  type: ADD_TODO,
  text: 'Build my first Redux app'
}
```

action type들을 위와 같이 선언해도 되고 많아졌을 경우에 모듈로 저장해서 아래와 같이 사용해도 됩니다.  

```js
//actionTypes에 type들을 정의해놓고 import 해서 사용 
import { ADD_TODO, REMOVE_TODO } from '../actionTypes'
```

***



### 1-2. Action Creaters

Action Creaters는 사실 redux에 있는 기능은 아닙니다. 뒤에 정리할 **Dispatch**라는 열차에 **Action**을 태워서 보내야하는데, 그때 Dispatch에 inline으로 action을 넣는 것이 불편하기 때문에 action객체를 return 해주는 함수를 만들어놓는 것입니다. 무조건 필요하다! 라는 것은 아닌 것 같습니다.  



Action을 return 해주는 함수라고 했기때문에, 아래와 같이 작성해보도록 하겠습니다.  

```js
function addTodo(text) {
  return {
    type: ADD_TODO,
    text
  }
}

```

`type`이 `ADD_TODO`이며 `text`를 담고 있는 Action을 리턴해주는 `addTodo`함수입니다. 이 함수 자체가 Action Creater가 되는 것입니다.  

***



### 1-3. Dispatch

Dispatch는 위에서 Action Creater로 return 해준 Action을 파라메터로 받아서 store의 reducer에게 넘겨주는 역할을 해주는 열차라고 생각하시면 편합니다.   

> store의 값을 변화 시키기 위해서는 action이 필요한데 그 action을 action creater가 만들어주고 action creater를 담은 dispatch열차가 store의 reducer에게 actiond을 전달해주면 reducer가 action의 type을 보고 그에 맞는 행동을 해주는 것입니다. 

위와 같이 생각할 수 있을 것 같습니다.  

그렇다면, 열차에 action을 담아서 호출하면 될 것 같으니 해보도록 하겠습니다.   

```js
//addTodo를 담고 reducer로 향하는 dispatch열차 
dispatch(addTodo(text))
```

위와 같이 작성하게 되면 매번 dispatch안에 action creator를 호출해서 사용해야하기 때문에 아래와 같이 함수를 만들어서도 사용할 수 있습니다.  

```js
const boundAddTodo = text => dispatch(addTodo(text))
boundAddTodo(text)
```

위와 같이 하면 손쉽게 dispatch를 호출할 수 있기 때문입니다.   

***



### 1-4. Reducer

reducer은 위에서 말했듯이 dispatch열차를 타고온 action의 type을 확인해서 그에 맞는 동작을 하는 곳입니다. 동작을 하기 때문에 function으로 작성이 됩니다.  

todoApp가 reducer가 되는 것입니다. reducer은 store의 state를 변경시켜야하기 때문에 `state`를 파라메터로 받고, dispatch를 타고온 `action`을 파라메터로 받습니다. 받아서 action의 type을  switch case문으로 조건을 걸어줍니다.  

```js
const initialState = {
  todos: []
}

function todoApp(state, action) {
  if (typeof state === 'undefined') {
    return initialState
  }

  return state
}
```

위 코드는 state가 들어오지 않았을 때 state에 initialState를 넣어주는 reducer입니다. 위의 코드는 아래와 같이 refactoring이 가능합니다.  

```js
function todoApp(state = initialState, action) {
  return state
}
```

지금은 type에 대해서 여러 케이스를 나눠 놓지 않았지만 아래와 같이 작성해서 case를 구분할 수 있습니다.  

```js
function todos(state = [], action) {
  switch (action.type) {
    case ADD_TODO:
      return [
        ...state,
        {
          text: action.text,
          completed: false
        }
      ]
    case TOGGLE_TODO:
      return state.map((todo, index) => {
        if (index === action.index) {
          return Object.assign({}, todo, {
            completed: !todo.completed
          })
        }
        return todo
      })
    default:
      return state
  }
}
```

위의 todos자체가 reducer가 되는 것입니다.

#### combineReducers

우리가 프로젝트르 만들다보면 글로벌 state로 User의 정보도 담고 싶고, todo의 정보 등등,,,, 많은 정보를 담고 싶어집니다. 그렇다면 여러개의 reducer가 필요하게 될 것입니다. 그럴 때 사용하는 것이 `combineReducers`입니다.  

```js
const todoApp = combineReducers({
  user,
  todos
})
```

위와 같이 작성하게 되면 todo reducer와 user reducer를 하나의 root reducer로 만들어주는 것입니다. 즉 하나의 부분을 하나의 reducer로 만들어주면 됩니다.  



todoReducer에서는 **AddTodo, ToggleTodo, deleteTodo, CompleteTodo**와 같은 동작을하고   

userReducer에서는 **login, logout, changePw**와 같은 type을 가지고 있어야 하는 것입니다.  



> 
>
>  #### redux**는 기본적으로 하나의 Store 멀티 Reducer의 형태를 갖는다.  
>
> 



### 1-5. 중간 요약

redux자체를 요약중이였는데, 요약하던걸 또 요약해보자면,  

> 중앙 state의 state에 접근하기위해서는 dispatch열차에 action을 태워보내야하고 dispatch는 action을 reducer에게 가져다 주면 reducer는 action의 type과 state를 가지고 해야할 동작을 해줍니다. 여기서 동작이란 state의 값을 변경시키는 것을 말합니다.  

위와 같이 정리를 해볼 수 있는데, 아직 저는 store를 만들지 않았기 때문에 store를 만들어보도록 하겠습니다. store는 react component의 최상위에 감싸져야 합니다.  



***



### 1-6. Store

store는 모든 컴포넌트에서 사용할 수 있는 Global State를 저장해놓는 저장소 입니다. 하지만 이 state는 엄격하게 관리해야 하기 때문에 **dispatch**라는 함수를 통해서만 state에 접근이 가능하다고 언급했습니다. 이제 reducer를 통해서 바꿔줄 store를 만들어보겠습니다.  

```js
import { createStore } from 'redux'
import todoApp from './reducers'
const store = createStore(todoApp)
```

**createStore**는 파라메터로 **Reducer**를 받아야합니다.  



```js
import {
  addTodo,
  toggleTodo,
  setVisibilityFilter,
  VisibilityFilters
} from './actions'

// Log the initial state
console.log(store.getState())

// Every time the state changes, log it
// Note that subscribe() returns a function for unregistering the listener
const unsubscribe = store.subscribe(() => console.log(store.getState()))

// Dispatch some actions
store.dispatch(addTodo('Learn about actions'))
store.dispatch(addTodo('Learn about reducers'))
store.dispatch(addTodo('Learn about store'))
store.dispatch(toggleTodo(0))
store.dispatch(toggleTodo(1))
store.dispatch(setVisibilityFilter(VisibilityFilters.SHOW_COMPLETED))

// Stop listening to state updates
unsubscribe()
```

***



### 1-7. redux 정리 

위와 같이 **Action**, **Action Creater** , **Dispatcher** , **Reducer** , **Store**의 개념만 이해가 된다면 redux를 작성하는데 크게 문제가 없을 것이라 생각합니다. 한가지 명심할 것은 위의 내용은 오로지 **Redux**의 내용을 정리한 것입니다. react에서 redux를 사용하기 위해서는 **Connect**, **mapStatetoProps**, **mapDispatchtoProps** 이 추가 되어야 합니다. 다음에는 react에서 redux를 사용할 수 있는 방법에 대해 정리해보도록 하겠습니다.  



***

