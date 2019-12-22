---
layout: post
title: "[React Hooks] ReactHooks 정리"
date: 2019-11-02-15:15:00
author: 한만섭
categories: reacthooks
tags: react hooks useState useInput useEffect useContext
---

- TOC
  
  {:toc}

## 정리할 내용

![1568095581110](../../../../assets/image/1568095581110.png)

React Hooks에 대해서 정리해보겠습니다. 리액트 훅은 리액트 16.8v에 나온 기능입니다. 나오게 된 배경은 함수형 컴포넌트에서도 클래스 컴포넌트처럼 상태관리를 할 수 있는 State를 사용할 수 있게 해주고, **componentDidMount** , **componentDidUpdate** 와 같은 라이프 사이클을 사용할 수 도 있게 해줍니다. 이처럼 훅의 장점은 원래 함수형에서 할 수 없었던 동작을 하게 해주는 것입니다.

그럼 주로 사용되는 훅을 정리해보도록 하겠습니다.

## 1. useState

가장 많이 사용하는 훅으로써 , 함수형 컴포넌트에서도 **State**를 지닐 수 있게 해주는 훅입니다. 함수형 컴포넌트에서 **State**를 사용하고 싶으시다면 사용하시면 됩니다.

간단한 사용법을 보여드리겠습니다.

```js
import React, { useState } from "react";

function Example() {
  // 새로운 state 변수를 선언하고, count라 부르겠습니다.
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  );
}
```

공식문서에 나와 있는 코드입니다. 해당 코드는 클릭을 하면 숫자를 증가시키는 코드입니다.

---

사용하기 위해서 import 해서 사용하시면 되고, react 에서 import하시면 됩니다.

```js
import React, { useState } from "react";
```

사용을 할 때는 아래와 같이 사용하시면 됩니다.

```js
const [count, setCount] = useState(0);
```

useState는 배열을 리턴해주는데 배열의 첫번째 인자는 value를 갖고 있고, 두번 째 인자는 value값을 **setting 해줄 수 있는 함수**를 가지고 있습니다.

클래스 컴포넌트 때 setState를 사용해서 state값을 변경하지 않으면 reRender되지 않았던 것 처럼 **set함수를 통해서 value를 변경해줄 때 reRender**를 해줍니다.

한 컴포넌트에서 여러개의 useState를 사용하는 것이 가능하기 때문에 필요하신 만큼 사용하시면 됩니다.

---

## 2. useEffect

useEffect는 클래스 컴포넌트에 존재했던 **라이프사이클**을 대신해줄 수 있는 훅입니다. 기존의 함수형 컴포넌트에서 렌더링 전 후 시점에 특정 동작을 하기 위해서 할 수 있는 방법이 없었는데, 해당 동작을 하고 싶다면 useEffect를 사용하시면 됩니다.

### 2.1 componentDidMount & componentDidUpdate

componentDidMount 와 componentDidUpdate의 동작을 해줄 수 있는 useEffect 사용법에 대해 설명 드리겠습니다. 샘플 코드는 아래와 같습니다.

```js
import React, { useState, useEffect } from "react";

function Example() {
  const [count, setCount] = useState(0);

  // Similar to componentDidMount and componentDidUpdate:
  useEffect(() => {
    document.title = `You clicked ${count} times`;
  });

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  );
}
```

---

사용법은 useState와 동일하게 react에서 import 해서 사용해야합니다.

```js
import React, { useState, useEffect } from "react";
```

import를 했다면 선언을 해줘야 하는데 useEffect는 아래와 같이 사용합니다.

```js
useEffect(() => {
  document.title = `You clicked ${count} times`;
});
```

보시면 파라메터로 아래처럼 생긴 함수를 받았습니다.

```js
() => {
  document.title = `You clicked ${count} times`;
};
```

이 함수가 바로 특정 상황일 때 호출될 함수입니다. 그렇다면 어떤 상황에서 호출이 될지는 useEffect의 두번째 파라메터를 통해서 정해줄 수 잇습니다. 지금은 두번 째 파라메터가 존재하지 않고, 그렇기 때문에 컴포넌트가 처음 생성될 때인 componentDidMount와 컴포넌트의 state가 업데이트되어서 발생하는 componentDidUpdate인 상황에서 함수가 호출됩니다.

---

 

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="4307878116"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>



### 2.2 componentDidMount

state가 달라질 때마다 호출하는 함수 말고 컴포넌트가 처음에 마운트 될 때만 함수를 호출하고싶다면 이 방법을 사용하시면 됩니다.

```js
import React, { useState, useEffect } from "react";

function Example() {
  const [count, setCount] = useState(0);

  // Similar to componentDidMount :
  useEffect(() => {
    document.title = `You clicked ${count} times`;
  }, []);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  );
}
```

위와 같이 두번째 파라메터로 []를 넣어주면 componentDidMount로 사용할 수 있습니다. 이게 도대체 왜 componentDidMount처럼 동작하는 것인지 궁금하실 것 같아 설명도 넣어놓도록 하겠습니다.

useEffect의 **두번째 파라메터**는 기본적으로 **어떤 state의 변화를 감지할 것인지 알려주는 곳**입니다. 한 컴포넌트에서 state는 여러개일 수 있고 그렇기 때문에 배열로 넣어줘야합니다.

처음에 componentDidMount & componentDidUpdate 에서 useEffect를 사용할 때는 두번 째 인자를 아예 넣지 않았기 때문에 모든 state의 변화를 다 감지할 수 있었던 것입니다. 그에 반해, `[]`를 넣어주게 되면 ''**어떠한 state의 변화도 감지하지 않을 것이다**'' 라는 의미입니다. 즉, 컴포넌트가 처음에 만들어질 때만 함수를 호출하게 됩니다.

---

### 2.3 특정 State의 변화만 감지하고 싶을 경우

위에서 사용한 것은 componentDidMount였다면, 이번에는 componentDidUpdate로 특정 state가 update되었을 때 호출 하는 방법입니다.

```js
useEffect(() => {
  document.title = `You clicked ${count} times`;
}, [count]); // Only re-run the effect if count changes
```

componentDidMount와 달라진 점은 `[]`안에 감지하고 싶은 State가 들어있다는 거서 뿐입니다. 사용되는 것을 보니 Vue의 Watch기능과 비슷한 역할을 하는 것 같습니다.

```js
useEffect(() => {
  document.title = `You clicked ${count} times`;
}, [count, name]);
```

위와 같이 작성하면 두가지 state에 대해서 감지가 가능합니다. 하지만 함수는 한가지 이기 때문에 여러 state를 감시하면서 각기 다른 동작을 하게 하고 싶다면, 아래와 같이 작성하면 됩니다.

```js
useEffect(() => {
  // count가 변했을 때 해야하는 동작
}, [count]);

useEffect(() => {
  // name가 변했을 때 해야하는 동작
}, [name]);
```

여기까지가 함수형 컴포넌트에서 state와 lifecycle을 사용하고 싶을 때 사용하는 훅입니다.

이제는 **중앙저장소 역할**을 할 수 있는 **useContext**와 react에서 **document.querySelector**와 같은 역할을 할 수 있는 **useRef**에 대해서 정리하고, useState를 사용해 useInput이라는 것을 만드는 것을 정리해보도록 하겠습니다.

---



## 3. useContext



`redux`,`mobx`와 같은 state management를 할 수 있는 Centext인데, React Hooks을 사용해 State management를 할 수 있는 기능입니다.   



------



### 3.1 왜 사용해야 하는가?

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



<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="4307878116"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>





### 3.2 Context 만들기

`context.js`파일 생성   

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

### 3.3 Context 사용하기 

그러면 이제 `Header.js`에서 `Context`를 사용해보도록 하겠습니다.  

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

```react
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
    <UserContext.Provider value={ user, setUser, fns: { loggedUserIn } }>
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

여기서도 `user`와 `loggedUserIn`이라는 것을 Context에서 가져오는 함수를 Context에 만들어 놨기 때문에 코드를 깔끔하게 작성할 수 있었습니다.  
여기서 `user`는 `{}`가 없는데 `{loggedUserIn}`은 있는 이유는 `useFunctions`는 함수 객체를 return 하기 때문에 그 중에서 하나인 loggedUserIn을 destructuring으로 받아온 것이고 `useUser`은 user만 return 하기때문에 저렇게 작성하는 것입니다.  

------

### 3.4 정리 

1. `context.js` 파일 만들기 
2. createContext를 이용해서 context만들기 (대문자 및 export const 주의)  
3. Provider 만들기 (대문자 주의 )
4. 필요한 태그 위에 넣어주면 children 들은 useContext를 통해서 사용가능 



***

