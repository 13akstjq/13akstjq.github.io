---
layout: post
title:  "[인스타클론코딩] [FrontEnd].2 - ApolloClient & ClientState & Query "
date:   2019-07-03-:09:52:00
author: 한만섭
categories: clonecoding
tags: react instagram
---

> ## Apollo-Client

로그인을 확인하기 위해 사용합니다. **apollo-boost**가 **Apollo client**를 쉽게 사용할 수 있게 해주는 라이브러리 입니다. 

***

> ### 1-1. Apollo Client 만들기 

클라이언트 프로젝트에서 `Apollo/Client.js`를 만들어서 내부에서 **apollo-boost**의 **ApolloClient**를 이용해서 
클라이언트를 쉽게 제작할 수 있습니다. 

![image](https://user-images.githubusercontent.com/46010705/60556335-c26d6c00-9d7b-11e9-8aeb-a336ef3bd80f.png)

**apollo-boost**의 **ApolloClient**를 import해서 새로운 **ApolloClient**를 사용합니다. uri는 서버의 주소인 `localhost:4000`를 사용합니다.  

```
import ApolloClient from "apollo-boost";

export default new ApolloClient({
  uri: "http://localhost:4000"
});

```

***

> ### 1-2. Apollo Provider로 App.js에 연결하기 

react-apollo의 **Apollo-Provider**를 사용해서 App.js에 Client를 서버와 연결해줘야 합니다. 이번에는 `react-apollo-hooks`를 사용해보려고 합니다. App.js에 Client가 제공되어야 하기 때문에 App.js에 Provider를 import해야합니다. ApolloProvider는 **client**를 필요로 하기 때문에 아래와 같이 작성해주면 됩니다.  

```
import React from "react";
import GlobalStyles from "../Styles/GlobalStyles";
import { ApolloProvider } from "react-apollo-hooks";
import { ThemeProvider } from "styled-components";
import Theme from "../Styles/Theme";
import AppRouter from "./AppRouter";
import Client from "../Apollo/Client";

export default () => (
  <ThemeProvider theme={Theme}>
    <ApolloProvider client={Client}>
      <GlobalStyles />
      <AppRouter isLoggedIn={!true} />
    </ApolloProvider>
  </ThemeProvider>
);


```

실행하고 **apollo-dev-tools**를 실행시키면 아래와 같이 뜹니다. 아직 서버를 켜지 않아서 그런 것입니다.  

![image](https://user-images.githubusercontent.com/46010705/60556699-7a4f4900-9d7d-11e9-8176-08636d34b123.png)  

서버 프로젝트로 가서 서버를 켜주고 나면 아래와 같이 동작합니다.   

![image](https://user-images.githubusercontent.com/46010705/60556772-b2ef2280-9d7d-11e9-83af-40f9522b48c8.png)  

***

> ### 1-3. clientState 만들기 

clientState를 만들기 전에 **ApolloClient**를 **index.js**로 옮기려고 합니다. 옮기는 이유는 
컴포넌트보다 **Query**를 먼저 생성하고 싶기 때문입니다. **clientState**는 App이 오프라인일때 발생합니다.  

처음에 **Router**의 argument로 사용되는 **isLoggedIn**을 `defaults`에서 정의하고 **Mutation**은 `resolvers`에서 정의합니다.  

**isLoggedIn**은 localstorage에 token이 저장되어있을 경우 **true** 아닐경우 **false**를 리턴합니다.  

**logUserIn** Mutation은 **token,cache**를 인자로 받아서  저장해줍니다.  
**logUserOut** Mutation은 **token,cache**를 초기화해줍니다.  

* **localState.js**
```
export const defaults = {
  isLoggedIn: localStorage.getItem("token") !== null ? true : false
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

* **Client.js**  

```
import ApolloClient from "apollo-boost";
import { defaults, resolvers } from "./LocalState";
export default new ApolloClient({
  uri: "http://localhost:4000",
  clientState: {
    defaults,
    resolvers
  }
});
```

***

> ## 2. Qeury
 
**Graphql**로 만들었던 bakcend의 Query를 호출하기 위해서는 client단에서 Query를 작성해놔야 합니다.  

Query를 만들기 위해서는 아래 import가 필요합니다.  

> ### gql

* import 

```
import {gql} from 'apollo-boost';
```

* Query 작성코드 
```
const QUERY = gql`
  {
    isLoggedIn @client
  }
`;
```

> ### useQuery로 Query 사용하기 

* import  

```
import {userQuery} from 'react-apollo-hooks';
```

* Query 사용코드   

```
const {data} = useQuery(QUERY);
```

***

> ### 최종 App.js 

  ```
  import Theme from "../Styles/Theme";
  import AppRouter from "./AppRouter";
  import { gql } from "apollo-boost";
  import { useQuery } from "react-apollo-hooks";

  const QUERY = gql`
    {
      isLoggedIn @client
    }
  `;

  export default () => {
    const { data } = useQuery(QUERY);
    return (
      <ThemeProvider theme={Theme}>
        <>
          <GlobalStyles />
          <AppRouter isLoggedIn={data.isLoggedIn} />
        </>
      </ThemeProvider>
    );
  };
  ```

로그인 상태일 경우 아래와 같이 Feed 페이지를 보여줍니다.   
![image](https://user-images.githubusercontent.com/46010705/60563356-6c59f200-9d96-11e9-979d-e3a95f9bc368.png)

로그아웃 상태일 경우 아래와 같이 Auth 페이지를 보여줍니다.   
![image](https://user-images.githubusercontent.com/46010705/60563398-a9be7f80-9d96-11e9-9014-dc4bb45e2b72.png)  

***

> ## 정리 

> ### Client  

apolloClient를 쉽게 제작할 수 있는 **apollo-boost**를 사용해서 Client를 만듭니다. **Client**는 uri와 clientState를 가질 수 있습니다. uri는 서버의 주소를 말하고, clientState는 App이 실행되기 전에 동작하는 State입니다. **defaults**와 **resolvers**를 담고 있는 **LocalState.js**를 만듭니다. **LocalState.js**에서는 **isLoggedIn** 변수를 localStroage의 **token**에 따라 true,false를 결정시켜주게 됩니다. client는 client prop을 필요로 하는 **apolloProvider**를 통해서 만들어집니다. **apolloProvider**은 **index.js**에서 작성합니다. 

> ### Query 

query는 apollo-boost의 **gql**을 사용해서 작성합니다. 작성한 Query는 **react-apollo-hooks**의 **useQuery**를 이용해서 사용할 수 있습니다. Query를 사용할 때는 API에 호출하지 못하도록 
`@client`를 해줘야 합니다.  



