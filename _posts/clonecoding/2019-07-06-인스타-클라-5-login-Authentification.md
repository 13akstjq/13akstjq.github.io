---
layout: post
title:  "[인스타클론코딩] [FrontEnd].5 - login Authentification 완성하기 "
date:   2019-07-06-14:47:00
author: 한만섭
categories: clonecoding
tags: react instagram
---


> ## To Do 

- [X] CreateAccount 
- [x] ConfirmSecret
- [X] @client Mutation 
　  

***

　  

> ## 1. CreateAccount 

계정을 만들기 위해선 우선 Query를 만들어야합니다. 저는 Qeury를 **Auth/AuthQueries.js**에 작성하고 있습니다.  

> ### Auth/AuthQueries.js  

```javascript
(...중략...)

export const CREATE_ACCOUNT = gql`
  mutation createAccount(
    $email: String!
    $username: String!
    $firstName: String!
    $lastName: String!
  ) {
    createAccount(
      email: $email
      username: $username
      firstName: $firstName
      lastName: $lastName
    )
  }
`;

(...중략...)
```  


Query를 만들었다면 AuthContainer에서 사용해야 합니다. 제가 만든 것은 Mutation이기 때문에 mutation을 사용하는 방법을 알아야 합니다. [react-apollo-hooks](https://github.com/trojanowski/react-apollo-hooks) 
에서 사용 방법을 확인할 수 있습니다. 간단하게 **useMutation**을 사용하고 **variables**로 인자를 전달하는 방식입니다.  

> ### Auth/AuthContainer.js  

```javascript
(...중략...)

  const createAccountMutation = useMutation(CREATE_ACCOUNT, {
    variables: {
      email: email.value,
      username: username.value,
      firstName: firstName.value,
      lastName: lastName.value
    }
  });
  
(...중략...)
```

전달 하는 variables를 주의해서 봐야할 것 같습니다. 회원가입을 할 경우 4개의 input에 입력한 value를 사용해서 회원가입을 하게 되는데, 그 때 
input의 value를 가져오기 위해 **useStat**를 이용해 **useInput**이라는 Hook을 사용했습니다. 여기서 useInput에 대해 간단하게 정리만 하도록 하겠습니다.  

> ### Hooks/useInput.js  

```javascript
import { useState } from "react";

export default (defaultValue, type = "text") => {
  const [value, setValue] = useState(defaultValue);

  const onChange = e => {
    const {
      target: { value }
    } = e;
    setValue(value);
  };
  return { value, onChange, type };
};

```

`useinput`에는 input 에 사용될 **defaultValue**와 input의 **type**을 입력받습니다. 또한 input에 사용자가 입력하는 것을 알기 위해 **onChange**라는 
메소드도 만들어 놓습니다.  

```javascript
const onChange = e => {
    const {
      target: { value }
    } = e;
    setValue(value);
  };
```

이 부분을 보게 되면 **e**라는 이벤트를 받게 됩니다. e는 아래와 같은 구조로 되어 있습니다.  

```javascript
e { 
  target : {
    value : {???}
    }
  }
```

여기서 value가 input의 value를 뜻하게 됩니다. 그렇기 때문에 e.target.value를 변경시켜주면 input의 value가 변경되게 되는 것입니다. 그래서 아래와 같은 함수를 작성하는 것이기도 합니다.  

```javascript
 const {
      target: { value }
    } = e;
```

***

이제 useInput을 사용해서 회원가입 form 이 제출될 경우 prisma에 회원가입을 요청하고 error처리를 해야합니다. 아래 코드는 회원가입 버튼을 눌렀을 때 
발생하는 submit 부분입니다.  

> ### Auth/AuthContainer.js  

```javascript
 const onSubmit = async e => {

(...중략...)

else if (action === "signUp") {
        console.log("singUp");
        if (
          email.value !== "" &&
          username.value !== "" &&
          firstName.value !== "" &&
          lastName.value !== ""
        ) {
          try {
            const { data: createAccount } = await createAccountMutation();
            if (createAccount) {
              toast.success("Success createAccount!! ");
              setTimeout(() => {
                setAction("logIn");
                email.value = "";
              }, 2000);
            }
          } catch (e) {
            toast.error(e.message);
          }
        }
      } 
      
(...중략...)
}
```

Input이 비어있지 않다면 위에서 만들었던 `createAccount()`를 실행시키게 됩니다. 여기서 가장 중요한점은 `await로 prisma의 응답을 기다리는 것`입니다. 
onSubmit함수에 async await를 사용하지 않을 경우 **response**가 오기전에 다음 코드를 진행하기 때문에 정상적인 진행이 되지 않습니다.  

await에 실패했을 경우 **catch**를 동작하고 await에서 에러가 발생하지 않았다면 toast로 성공 메세지를 띄어주고 `setAction()`으로 보여주는 화면을 
전환 해줍니다.  

한가지 의문이 있다면 이렇게 Action을 변경하는 방식으로 보여지는 page를 변경시켜준다면 새로고침을 할경우 데이터가 날라가게 되는데 주소에 `/signUp`을 
사용하는 방식이 좋은지 `setAction('signUp')이 좋은지는 추가적으로 공부해봐야 알 수 있을 것 같습니다.  
　  
   
***

　  
> ## ConfirmSecret   

confirmSecret도 같은 방식으로 제작할 수 있습니다.  

* Queries에 Mutation만들기  

```javascript
export const CONFIRM_SECRET = gql`
  mutation confirmSecret($secret: String!, $email: String!) {
    confirmSecret(secret: $secret, email: $email)
  }
`;
```

**confirmSecret**은 **email**과 **secret**을 argument로 받습니다. 혹시 까먹었다면 아래화면에서 확인할 수 있습니다.  

![image](https://user-images.githubusercontent.com/46010705/60752667-73d20300-a003-11e9-86a0-20f6cddf8258.png)  

signUp을 했던 방식과 똑같이 Container에서 불러와 사용합니다. 사용할 때는 useMutation을 사용하겠죠.  

```javascript
const confirmSecretMutation = useMutation(CONFIRM_SECRET, {
    variables: {
      email: email.value,
      secret: secret.value
    }
  });
```

그 다음은 인증하기 버튼을 눌렀을 때 발생하는 onSubmit함수에서 현재 Action이 confirm이면 실행을 해야하기 때문에 아래와 같이 작성해줍니다.  

```javascript
(...중략...)

 else if (action === "confirm") {
        try {
          const { data: confirmSecret } = await confirmSecretMutation();
          console.log(confirmSecret);
          if (confirmSecret) {
            toast.success("Success confirm secret!! ");
          } else {
            toast.error("Paste your correct scret!!");
          }
        } catch {
          toast.error("Paste your correct scret!!");
        }
      }
      
(...중략...)
```

위 코드에서도 보다시피 `await`를 사용해서 동기방식으로 기다린 후에 data의 confirmSecret을 confirmSecret변수에 집어 넣습니다. 변수의 상태에 따라서 토스트 메세지를 보여줍니다.  
지금과 같은 경우는 confirmSecret의 return 은 token이기 때문에 이제 token과 cache를  localStorage에 저장해주는 Mutation이 필요하다는 것을 알 수 있습니다.  

현재 제 프로젝트에서 Auth라는 page를 보여주고 있는 곳은 아래 코드입니다.  

```javascript
import React from "react";
import { ToastContainer } from "react-toastify";
import "react-toastify/dist/ReactToastify.css";
import GlobalStyles from "../Styles/GlobalStyles";
import styled, { ThemeProvider } from "styled-components";
import Theme from "../Styles/Theme";
import AppRouter from "./AppRouter";
import { gql } from "apollo-boost";
import { useQuery } from "react-apollo-hooks";
import Footer from "./Footer";

const QUERY = gql`
  {
    isLoggedIn @client
  }
`;

const Wrapper = styled.div`
  display: flex;
  flex-direction: column;
  justify-content: center;
  margin: auto;
  max-width: 935px;
  width: 100%;
  height: 100vh;
`;
export default () => {
  const {
    data: { isLoggedIn }
  } = useQuery(QUERY);
  return (
    <ThemeProvider theme={Theme}>
      <Wrapper>
        <GlobalStyles />
        <AppRouter isLoggedIn={isLoggedIn} />
        <Footer />
        <ToastContainer position={"bottom-left"} />
      </Wrapper>
    </ThemeProvider>
  );
};

```

App.js에서 `isLoggedIn`변수를 Query를 통해서 불러오고 있습니다. Query는 아래와 같이 @client에서 isLoggedin변수를 가져오고 있습니다.  

```javascript
const QUERY = gql`
  {
    isLoggedIn @client
  }
`;
```

그렇다면 `isLoggedIn`변수는 어떻게 결정되는지 알아봐야겠죠? 아래 코드를 보면 localStorage의 **token**이 존재하면 true 아니면 false를 리턴해주고 있습니다. 현재는 당연히 localStorage에 **token**이 없기 때문에 **false**를 리턴해줍니다.  

Apollo/LocalState.js
```javascript
export const defaults = {
  isLoggedIn: Boolean(localStorage.getItem("token")) || false
};

export const resolvers = {
  Mutation: {
    logUserIn: (_, { token }, { cache }) => {
      localStorage.setItem("token", token);
      cache.writeData({
        data: {
          isLoggedIn: true
        }
      });
      return null;
    },
    logUserOut: (_, __, { cache }) => {
      localStorage.removeItem("token");
      window.location.reload();
      return null;
    }
  }
};

```

그렇다면 false일 경우 `<AppRouter>`는 어떤 페이지를 보여질지 확인 해봐야겠죠?  

```javascript
import { HashRouter as Router, Route, Switch } from "react-router-dom";
import React from "react";
import Feed from "../Routes/Feed";
import Auth from "../Routes/Auth";
import PropType from "prop-types";
const LogedInRouter = () => (
  <>
    <Route exact path="/" component={Feed} />
  </>
);
const LogedOutRouter = () => (
  <>
    <Route exact path="/" component={Auth} />
  </>
);

const AppRouter = ({ isLoggedIn }) => (
  <Router>
    <Switch>{isLoggedIn ? <LogedInRouter /> : <LogedOutRouter />}</Switch>
  </Router>
);

AppRouter.prototype = {
  isLoggedIn: PropType.bool.isRequired
};

export default AppRouter;

```

* logedInRouter : 로그인이 되어있을 경우 보여지는 Router 현재는 **Feed**Component를 보여주고 있네요.  
* logedOutRouter : 로그인이 되어있지 않을 경우 보여지는 Router 현재는 **Auth** Component를 보여주고 있어요.  

* AppRouter : isLoggedIn이라는 argument를 받아서 true일경우 **logedInRouter** false일경우 **logedOutRouter**를 보여줍니다. 정리를 해보자면  

현재 localStorage에 token은 저장되어있지 않기 때문에 `isloggedIn = false` false를 받은 `<AppRouter>`는 `logedOutRouter`를 경로로 보여줍니다. 
`logedOutRouter`는 Auth컴포넌트를 보여주고 있고, 그렇기 때문에 로그인, 회원가입, 인증을 하는 Auth를 보여주게 되겠죠.  

자, 그러면 이 얘기를 왜 했느냐하면 비밀번호 인증까지 마쳤기 때문에 이제 로그인이 성공해서 Feed를 보여줘야합니다. 그렇다면 발급 받은 token을 
LocalStorage에 넣어주면 isLoggedIn이 true가 될 것이고 AppRouter는 logedInRouter를 경로로 지정할 것입니다.  

이것을 해보려고 합니다. 지금까지 Query를 만든 방식과는 약간 다릅니다. 왜냐하면 client에서 LocalState에 만든 Mutation을 사용하기 때문이죠.  
　  

***

　  

> ## @Client Mutation사용하기  

token도 얻었겠다. 예전에 만들었던 LocalState에 정의했던 **Mutation**만 사용하면 됩니다.  

그렇다면 AuthQueries에 또 정의를 해야겠죠?  아래와 같이 정의해줍니다.  
다른부분은 같지만 마지막에 `@client`를 사용하는 것이 다릅니다. 정확히 이해하지는 못했지만 아마 client의 Mutation을 사용한다는 의미인것 같습니다.  개발은 직감도 중요하다고 생각하기 때문에 일단 넘어가고 나중에 정리하도록 하겠습니다.  

```javascript
export const LOCAL_LOG_IN = gql`
  mutation logUserIn($token: String!) {
    logUserIn(token: $token) @client
  }
`;
```

```javascript
const logInMutation = useMutation(LOCAL_LOG_IN);
```

variables를 위에서 넣지 않고 여기서 넣은 이유는 token은 confirmSecret을 하기전까지 없는 값이기 때문입니다.  

```javascript
await logInMutation({ variables: { token } });
toast.success("Success confirm secret!! ");
```


