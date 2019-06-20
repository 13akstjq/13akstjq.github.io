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

  1. react-router 설치하기 
  
    ```
    npm add react-router-dom
    ```
    
  2. react Apollo 설치하기 
  
  react-apollo는 [github](https://github.com/apollographql/react-apollo)에 있는 커맨드를 입력해 설치를 해야합니다.  
    **Apollo-boost**는 GraphQL Client를 사용하기위해 모든 것을 대신 셋업해주는 라이브러리라고 생각하시면 됩니다.    
    
    ```
    # installing the preset package (apollo-boost) and react integration
    npm install apollo-boost react-apollo graphql-tag graphql --save

    # installing each piece independently
    npm install apollo-client apollo-cache-inmemory apollo-link-http react-apollo graphql-tag graphql --save
    ```
    
　  
   
 ***
 
 　  
> ### 
 
