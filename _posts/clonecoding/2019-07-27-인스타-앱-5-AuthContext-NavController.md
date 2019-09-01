---
layout: post
title: "[인스타클론코딩] [App].5- AuthContext, NavController만들기 "
date: 2019-07-27-16:17:00
author: 한만섭
categories: clonecoding
tags: react instagram app Context Controller
---

* TOC
{:toc}





> ## AuthContext 



* `AuthContext`

  `createContext`를 사용해서 만들 수 있음.  

  ```js
  export const AuthContext = createContext();
  ```

  

* `AuthProvider`

  ```react
  export const AuthProvider = ({ isLoggedInProp, children }) => {
    const [isLoggedIn, setIsLoggedIn] = useState(isLoggedInProp); // true,false를 구분하기 위해 null로 초기화
  
    const logUserOut = async () => { 
      try {
        await AsyncStorage.setItem("isLoggedIn", "false");
        console.log("로그아웃");
        setIsLoggedIn(false);
      } catch (error) {
        console.log(error);
      }
    };
  
    const logUserIn = async () => {
      try {
        await AsyncStorage.setItem("isLoggedIn", "true");
        console.log("로그인");
  
        setIsLoggedIn(true);
      } catch (error) {
        console.log(error);
      }
    };
  
    return (
      <AuthContext.Provider value={{ isLoggedIn, logUserIn, logUserOut }}>
        {children}
      </AuthContext.Provider>
    );
  };
  
  ```

  login logout 함수는 `async` `await`로 만들어야합니다.  `AsyncStorage`가 비동기 함수이기 때문에 기다려줘야합니다.  

  `<AuthContext.Provider value={}>`  value안에 객체가 들어가야하기 때문에 하나만 들어가면 `{}`가 필요 없지만 위와 같이 여러 함수가 들어가게 될 경우 `{}`를 사용해야합니다.  



#### AuthContext를 나눈 이유 ?   

Context를 사용하는 이유는 함수나 변수를 많은 컴포넌트에서 사용해야할 때 `context`를 만듭니다.  

지금과 같은 경우에도 login, logout`같은 경우 다양한 컴포넌트에서 호출을 하게 될 것이고 그것을 하나의 Context인 Authcontext`라는 파일로 만들어서 사용하는 것입니다. AuthContext에서 제공하는 state와 function을 Navigation을 Control해야하는 NavController에서 사용할 것입니다.  

이렇게 하게 되면 기존에 무거웠던 `App.js`가 조금은 가벼워 질 것 같습니다.  







***



> ## NavContainer  

App.js에 있던 것들을 `NavContainer`로 옮겨줍니다. `NavContainer`가 하는 일은 login 화면을 보여줄지 logout화면을 보여줄지 결정하는 곳입니다.  아직 Navigation에 대해서 공부하지는 않았지만 React에서의 Router와 Navigation이 비슷한 역할을 하는 것 같습니다. 그래서 NavigationContainer은 앱화면에서 보여주어야할 페이지를 결정해주는 역할을 하는 것 같습니다.  



AuthContext에서 제공해주는 로그인을 했는지 알려주는 state와 로그인,로그아웃 해주는 함수가 있습니다. 이것들을 사용해서 isLoggedIn이 true일 경우에 logout버튼을 보여주고 isLoggedIn이 false일 경우 login버튼을 보여주게 됩니다.   지금까지는 Button 을 보여주지만 각각의 Navigation을 보여주게될 것 같습니다.  



***



