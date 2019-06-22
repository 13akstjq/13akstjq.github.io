---
layout: post
title:  "[인스타클론코딩] [Server].5 - prisma query 익숙해지기"
date:   2019-06-22-19:12:00
author: 한만섭
categories: clonecoding
tags: graphql prisma 
---


> ### 유저 생성 
  
  ```
  mutation {
  createUser(data:{username :"byel" , email :"byel@naver.com"}){
    id
  }
  }
  ```
  
> ### 유저들 정보 검색 

  ```
  {
  users{
    username
  }
  }
  ```
  
  
> ### 유저정보 업데이트 

  ```
  mutation {
  updateUser(data :{lastName :"eun",firstName :"byel"}where :{id :"cjx7jfr1mpo3w0b121w1bgm6x"}){
    username
  }
  }
  ```
  
> ### 특정 유저 선택 - **where**
  
  following 같이 [User]로 된 것은 User의 속성을 또 입력해줘야 합니다. 
  ```
  {
  users(where :{id :"cjx7jfr1mpo3w0b121w1bgm6x"}){
		username
    firstName
    lastName
    following{
      id
    }
    followers{
      id
    }
  }
  } 
  ```
  
> ### following 추가 **connect**이용 
  
  **connect** : 기존회원정보 연결
  **disconnect** : 기존회원정보 끊음 
  
  ```
  mutation {
  updateUser(data : {following : {
    connect : {id :"cjx7izjhyblr90b422eyooqnw"}
  }}where :{id :"cjx7jfr1mpo3w0b121w1bgm6x"}){
    username
    lastName
    firstName
    email
    following {
      id
      username
    }
  }
  }
  ```
