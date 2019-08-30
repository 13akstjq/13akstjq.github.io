---
layout: post
title:  "[redux] react에서 redux 사용하기"
date:   2019-08-30 19:41:00
author: 한만섭
categories: redux
tags: react redux action actioncreator dispatch connect mapstatetoprops mapdispatchtoprops 
---



[TOC]





## 서론 

redux는 상태관리를 할 수 있는 라이브러리입니다. **JQuery**, **Vue**, **React**, **Vanilla JS**외에도 많은 곳에서 어디서든 사용할 수 있는 **상태관리 라이브러리**입니다. 그렇다면 react에서 redux를 사용하고 싶다면 추가적으로 해줘야하는게 있습니다. 오늘은 그것에 대해 정리를 해보려고 합니다.  

***



## 1. React-redux

[React-redux](https://react-redux.js.org/)는 react에서 redux를 사용할 수 있도록 만들어놓은 라이브러리 입니다. 이것을 사용해서 외부에서 만든 redux를 react에 **연결**해보도록 하겠습니다.  

 

### 1-1. connect

위에서 **연결**이라는 말을 했다시피 redux를 react에 연결시킨다고 생각하면 편합니다. 특정 컴포넌트에게 store의 특정 state,dispatch를 연결해줘야 그 컴포넌트에서 사용할 수 있습니다.  connect를 사용하기 위해서는 아래의 두가지 개념이 필요합니다.  

#### mapStateToProps

​	- 이름 그대로 **State**(store의 state)를 **Props**(component의 Props)로 넘겨주는 map

#### mapDispatchToProps

​	- 이름 그대로 **Dispatch**(store의 Dispatch)를 **Props**(component의 Props)로 넘겨주는 map



위의 두가지가 있어야 컴포넌트에서 store의 state와 Dispatch를 사용할 수 있습니다. 위의 두가지를 해당 컴포넌트에 넘겨줘야합니다.  

```js
import { connect } from 'react-redux'
import Link from '../components/Link'

// store의 state를 컴포넌트의 props에게 전달 
const mapStateToProps = state => {
  return {
    todos: getVisibleTodos(state.todos, state.visibilityFilter)
  }
}

// store의 dispatch를 컴포넌트의 props로 전달
const mapDispatchToProps = dispatch => {
  return {
    onTodoClick: id => {
      dispatch(toggleTodo(id))
    }
  }
}

// 기존에 만들었던 Link컴포넌트 + store의 state,dispatch를 해서 `NavLink`라는 컴포넌트를 만듬. 
const NavLink = connect(
  mapStateToProps,
  mapDispatchToProps
)(Link)

export default NavLink
```

위와 같이 코드를 작성해서 연결할 수 있습니다. 위와 같이 코드를 작성한다는 것은 기존에 존재하는 `Link`컴포넌트에 store의 state,dispatch를 더해서 `NavLink`라는 컴포넌트를 만들어서 사용하겠다는 것 입니다. 위와 같이 작성하게 될 경우 컴포넌트의 재사용성이 뛰어날 것으로 생각됩니다. 위와 같이 연결해주기 전에 해줘야 하는 것이 있습니다.  



### 1-2. Provider 

`index.js`

```js
import React from 'react'
import { render } from 'react-dom'
import { Provider } from 'react-redux'
import { createStore } from 'redux'
import todoApp from './reducers'
import App from './components/App'

const store = createStore(todoApp)

render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
)
```

react의 root 컴포넌트에 react-redux에서 제공하는 provider를 넘겨준 후에 연결을 해줘야 합니다.  



***



### 1-3. react-redux 정리 

connect를 사용하는 것이 굉장히 귀찮다고 생각했던 와중에 redux를 사용하면서 hook을 사용하게 되면 useSelector를 통해서 connect를 대신할 수 있다는 사실을 알게 되었습니다. 다음은 react + redux + hook 을 사용하는 것을 정리해보려고 합니다.  





