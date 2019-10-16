---
layout: post
title: "[Portfolio][04] rendering횟수를 줄이기위한 Context재설계"
date: 2019-09-10-02:13:00
author: 한만섭
categories: portfolio
tags: portfolio React
---

- TOC
  
  {:toc}

## 정리할 내용

리액트 프로젝트를 점차 고도화 시키면 시킬수록 사이트가 느려지는 느낌을 받았는데, 역시 제가 **리액트의 라이프 사이클**과 **Context**가 어떤 방식으로 호출되는지 제대로 알지 못해서 발생했던 이슈였습니다. 아래를 보면 휠을 할 때 굉장히 느린 느낌을 받을 수 있었는데, 거의 모든 컴포넌트들이 **Rerender**되고 있던 것을 볼 수 있습니다.

![Honeycam 2019-09-10 00-45-08](../../../../assets/image/Honeycam 2019-09-10 00-45-08.gif)

## 문제 원인

### 1. 하나의 Context에 모든 State를 선언 했던 것

제가 작성한 방식은 아래와 같이 AppContext에 모든 전역 State를 다 넣어놨습니다. 이렇게 될 경우 AppContext의 state가 하나라도 변경되게 되면 나머지 State를 가져다 쓰고 있는 컴포넌트도 render가 되는 것을 확인할 수 있었습니다. (정확한 사항이 아니기 때문에 다시 실험해봐야함. )

```js
return (
  <AppContext.Provider
    value={{
      scrollIndex,
      setScrollIndex,
      selectedProject,
      setSelectedProject,
      isSideOpen,
      setIsSideOpen,
      projects,
      posts,
      setPosts,
      setProjects,
      isAuthOpen,
      setIsAuthOpen,
      isChatOpen,
      setIsChatOpen,
      isJobLess,
      myProfile
    }}
  >
    {children}
  </AppContext.Provider>
);
```

### 2. 상위 컴포넌트가 render되면 하위 컴포넌트는 자동으로 render된다.

어떠한 컴포넌트가 render되면 그 하위 컴포넌트들은 자동으로 render가 되는 것을 생각하지 않고 프로그램을 작성했는데, 이렇게 될 경우 Page인 **Home**에서 useState를 통해 state의 값을 setting 하게 되면 **Home**이 Render될 때 Home에 그려지는 모든 Component가 다시 그려지게 됩니다.

좀 더 자세히 설명하자면, Wheel을 하게되면 Home컴포넌트에서 현재 선택된 Project의 번호를 새로 set하게 되고, **set**을 호출하는 순간 모든 컴포넌트들이 다시 그려지게 됐습니다.

그래서 Context를 사용하고 있기 때문에 하위 컴포넌트에서 직접 Context의 값을 가져와서 사용했고, 이렇게 하게 될 경우 Contex의 State를 가져다가 쓰고 있는 컴포넌트만 다시 render가 되는 구조가 됩니다.



---

## 주의할 점

설계를 할 때 각 컴포넌트의 **라이프사이클**을 이해하고 해당 컴포넌트가 언제 **rerender**되는지 생각한 후에 설계를 해야하는 것 같습니다.

또한, 컴포넌트가 **rerender**되는 것을 **console**로 확인한채로 개발하면 미리 이런 일을 방지할 수 있을 것 같습니다.



***



## 해결방법

- 우선 하나로 사용하던 Context를 기능별로 나눠줬습니다. 

![image](https://user-images.githubusercontent.com/46010705/66912781-32afad00-f04e-11e9-9861-39e916a6cb80.png)

- `AuthContext.js`

  ```js
  import React, { createContext, useState } from "react";
  
  //컨텍스트 생성
  export const AuthContext = createContext();
  
  const AuthContextProvider = ({ children }) => {
    const [isAuthOpen, setIsAuthOpen] = useState(false);
  
    return (
      <AuthContext.Provider
        value={{
          isAuthOpen,
          setIsAuthOpen
        }}
      >
        {children}
      </AuthContext.Provider>
    );
  };
  
  export default AuthContextProvider;
  
  ```

  

- AppContext.js`에서 모든 컨텍스트들을 받은 후에 내보내는 방법을 사용했습니다.  

  `AppContext.js`

  ```js
  import React from "react";
  import AuthContextProvider from "./AuthContext";
  import BlogContextProvider from "./BlogContext";
  import ProjectContextProvider from "./ProjectContext";
  import ChatbotContextProvider from "./ChatbotContext";
  import SideBarContextProvider from "./SideBarContext";
  import UserContextProvider from "./UserContext";
  
  const AppContext = ({ children }) => {
    return (
      <AuthContextProvider>
        <BlogContextProvider>
          <ProjectContextProvider>
            <ChatbotContextProvider>
              <SideBarContextProvider>
                <UserContextProvider>{children}</UserContextProvider>
              </SideBarContextProvider>
            </ChatbotContextProvider>
          </ProjectContextProvider>
        </BlogContextProvider>
      </AuthContextProvider>
    );
  };
  
  export default AppContext;
  
  ```

- `App.js`

  ```js
  import React from "react";
  import Router from "./Router";
  import GlobalStyle from "./Styles/GlobalStyle";
  import { ThemeProvider } from "styled-components";
  import Theme from "./Styles/Theme";
  import AppContext from "./Context/AppContext";
  
  function App() {
    return (
      <div className="App">
        <AppContext>
          <ThemeProvider theme={Theme}>
            <>
              <GlobalStyle />
              <Router />
            </>
          </ThemeProvider>
        </AppContext>
      </div>
    );
  }
  
  export default App;
  
  ```