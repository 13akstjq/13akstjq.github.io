---
layout: post
title:  "[React] 리액트 Hooks (1) - useContext "
date:   2019-07-24-00:15:00
author: 한만섭
categories: react
tags: react hooks useContext mobx redux
---

> ## useContext란?  



`redux`,`mobx`와 같은 state management를 할 수 있는 Centext인데, React Hooks을 사용해 State management를 할 수 있는 기능입니다.   



------

> ### 왜 사용해야 하는가?

`App.js`  

```javascript
import React from "react";
import Screen from "./Screen";
function App() {
  return <Screen name="mansub" />;
}
export default App;
```

`Screen.js`  

```javascript
import React from "react";
import Header from "./Header";
export default ({ name }) => {
  return <Header name={name} />;
};
```

`Header.js`  

```javascript
import React from "react";
export default ({ name }) => {
  return <span>your name is {name}</span>;
};
```

와 같이 구성된 react 프로젝트가 있다고 가정해봅시다. `App.js` -> `Screen.js` -> `Header.js`로 name이라는 props를 전달해서 reaf component인 Header컴포넌트에서 `name`을 사용하고 있습니다. 위의 프로그램에서는 아래와 같은 문제점이 있습니다.  

1. Screen.js에서는 name을 사용하지도 않는데 넘겨주고 있다.   
2. propTypes와 같은 코드를 불필요하게 반복해야한다.   
3. `name`의 데이터가 어디서 온지 쉽게 알 수 없다.    

위와 같이 데이터를 불필요하게 하위 컴포넌트에게 전달하는 상황을 막기 위해서 `name`을 저장소에서 꺼내서 사용하면 편리하지 않을까? 라는 생각을 통해`Context`개념을 사용하는 것입니다.  
