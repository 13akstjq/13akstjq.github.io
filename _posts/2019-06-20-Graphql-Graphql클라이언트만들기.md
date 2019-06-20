---
layout: post
title:  "[Graphql] Graphql 클라이언트 만드는 방법"
date:   2019-06-20-19:04:00
author: 한만섭
categories: graphql
tags: graphql apollo boost
---

> ## GraphQL 클라이언트 부분 만드는 방법 
  **Graphql**을 통해서 RESTAPI와 같은 API서버를 만들었다면 서버에게 데이터를 요청할 클라이언트(front)부분이 필요합니다. 오늘은 클라이언트 부분을 
  만드는 방법을 정리해보려 합니다. Apollo를 사용해서 프로젝트를 만드려고 합니다. 
  
> ### 프로젝트 만들기 
  * 우선 프로젝트를 만들어야 합니다. 기본 CRA 프로젝트 setting 하는 방법은 항상 동일합니다.  
      * 콘솔에서 **CRA**프로젝트 만들어주기  
      *  **Git Repository**만들기  
      * CRA프로젝트에서 필요없는 파일들 **지우기**  
      * CRA와 Git **연동하기**  
  이렇게 하면 기본적인 프로젝트 생성은 완성입니다.  
  　  
     
***

　  

> ### 라이브러리 설치하기 

  * react-router 설치하기 
  
    ```
    npm add react-router-dom
    ```
  
  * react Apollo 설치하기 
  
    react-apollo는 [github](https://github.com/apollographql/react-apollo)에 있는 커맨드를 입력해 설치를 해야합니다.  
    **Apollo-boost**는 GraphQL Client를 사용하기위해 모든 것을 대신 셋업해주는 라이브러리라고 생각하시면 됩니다.    

    ```
    # installing the preset package (apollo-boost) and react integration
    npm install apollo-boost react-apollo graphql-tag graphql --save

    # installing each piece independently
    npm install apollo-client apollo-cache-inmemory apollo-link-http react-apollo graphql-tag graphql --save
    ```
    
　  
   
 ***
 
 　  
> ### apolloClient.js 만들기 
    
   * ApolloClient.js
      **uri**에 적는 주소는 **GraphQL**서버의 주소입니다. 저는 localhost:4000에 켜놨기 때문에 아래와 같이 작성했습니다. 
      ```
      import ApolloClient from 'apollo-boost';

      const client = new ApolloClient ({
          uri :"http://localhost:4000/"
      })

      export default client;
      ```
      
  * App.js
    ```
    import React ,{Component}from 'react';
    import {ApolloProvider} from 'react-apollo';
    import client from './apolloClient';

    class App extends Component {
      render(){
        return (
          <ApolloProvider client={client}>
            <div className="App"/>
          </ApolloProvider>
        );
      }
    }



    export default App;

    ```
 
    **npm start**로 실행시키면 
    ![image](https://user-images.githubusercontent.com/46010705/59844159-1dbf5780-9395-11e9-928b-beb2c9ddd0de.png)
    위와 같은 화면이 나올 것 입니다. 화면에서 사용한 tool은 [apollo dev tools](https://chrome.google.com/webstore/detail/apollo-client-developer-t/jdkknkkbebbapilgoeccciglkfbmbnfm)에서 설치할 수 있습니다.  
    
    
