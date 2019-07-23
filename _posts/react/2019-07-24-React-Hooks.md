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

> ### Context 생성하기 
>
>  `context.js`파일 생성   

```javascript
import React, { useState, createContext } from "react";
// user의 정보를 담아줄 수 있는 Context 생성
export const UserContext = createContext();
// userContext를 하위 컴포넌트에게 전달해줄 Provider생성
const UserContextProvider = ({ children }) => { 
  const [user, setUser] = useState({
    name: "mansub",
    isLoggedIn: false
  });
  return (
    <UserContext.Provider value={{ user }}>{children}</UserContext.Provider>
  );
};
export default UserContextProvider;
```

`UserContext` 혹은 `UserContextProvider`같은 경우 대문자로 시작하지 않을 경우 `Component`형식에 어긋난다는 에러를 발생시킵니다.  
위와 같이 모든 컴포넌트가 사용할 수 있는 `context`를 만들었습니다. 저는 user정보가 필요하기 때문에 `userConext`를 만들고 하위 컴포넌트에게 전달해 줄 수 있는 `userContextProvider`를 만들었습니다.  

------

> ### Context사용하기 
>
> 그러면 이제 `Header.js`에서 `Context`를 사용해보도록 하겠습니다.  

`Header.js` 

```javascript
import React, { useContext } from "react";
import { UserContext } from "./context";
export default () => {
  const {
    user: { name }
  } = useContext(UserContext);
  return <span>your name is {name} </span>;
};
```

`UserContext` import 해서 사용하는데 `value`로 넘겨준 user를 사용하면 됩니다.  
이번에는 로그인을 체크하는 메소드를 만들어보겠습니다.  
그런데 매번 `useContext`를 사용해서 컴포넌트 내에서 코드를 작성해야 하는 것은 별로 보기 좋아보이지는 않을 것 같습니다. 그래서 `Context`내에서 `useContext`를 통해서 원하는 것을 return 해주는 함수를 작성해보겠습니다.  

`context.js`

```javascript
import React, { useState, useContext, createContext } from "react";
// user의 정보를 담아줄 수 있는 Context 생성
export const UserContext = createContext();
// userContext를 하위 컴포넌트에게 전달해줄 Provider생성
const UserContextProvider = ({ children }) => {
  const [user, setUser] = useState({
    name: "mansub",
    isLoggedIn: false
  });
  const loggedUserIn = () => setUser({ ...user, isLoggedIn: true });
  return (
    <UserContext.Provider value={{ user, setUser, fns: { loggedUserIn } }}>
      {children}
    </UserContext.Provider>
  );
};
// use User
export const useUser = () => {
  const { user } = useContext(UserContext);
  return user;
};
// use functions
export const useFunctions = () => {
  const { fns } = useContext(UserContext);
  return fns;
};
export default UserContextProvider;
```

`Header.js`

```javascript
import React from "react";
import { useUser } from "./context";
export default () => {
  const user = useUser();
  return <span>your name is {user.name} </span>;
};
```

`Header`에서 `useContext`를 호출하지 않게 되니 좀더 코드를 깔끔하게 작성할 수 있습니다.  

`Screen.js`

```javascript
import React from "react";
import Header from "./Header";
import { useFunctions, useUser } from "./context";
export default () => {
  const { loggedUserIn } = useFunctions();
  const user = useUser();
  return (
    <>
      <Header />
      <span>you are {user.isLoggedIn ? "loggedIn" : "annonymous"} </span>
      <button onClick={() => loggedUserIn()}>로그인</button>
    </>
  );
};
```


