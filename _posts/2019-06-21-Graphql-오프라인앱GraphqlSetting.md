---
layout: post
title:  "[Graphql] 오프라인앱(1) - GraphqlSetting"
date:   2019-06-21-17:32:00
author: 한만섭
categories: graphql
tags: graphql 
---



> ### 정리할 내용 
  * redux를 사용해서 Global State를 관리하는 것이 아니라 Apollo를 통해서 관리하는 방법을 정리해보려고 합니다.  
  
　  
 
 ***
 
 　  
    

> ### 프로젝트 셋팅
  * ### Git과 react기본프로젝트 셋팅
    
    Gitrepo를 만드는 점은 동일하지만 이번에는 create-react-app 을 이용해서 프로젝트를 만드는 것이 아니라, npx create-react-app 으로 
    프로젝트를 만들어주면 됩니다.  
    
  * ### 라이브러리 설치  
    
    ```
    npm add apollo-cache-inmemory apollo-client apollo-link graphql react-apollo styled-components apollo-link-state react-apollo
    ```
