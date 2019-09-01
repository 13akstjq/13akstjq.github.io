---
layout: post
title:  "[인스타클론코딩] [Server].18 - Computed 만들기 ,AmIFollowing(1)  "
date:   2019-06-26-21:23:00
author: 한만섭
categories: clonecoding
tags: instagram post fragment
---

* TOC
{:toc}

> ## 정리할 내용 

기존 모델에 생성하지 않은 속성이 존재할때 추가해줘야 하는 경우가 있습니다. 예를 들어, **User**모델에서 **FirstName**과 **LastName**을 이용해서
**FullName**을 만들어야하는 경우입니다. 이럴 때 해당 **Model**에 Computed를 만들어서 필요한 것을 정의하는 것입니다.  
　  

***

　  
> ### code

* User/computed.js 
  
  ```
  import { prisma } from '../../../generated/prisma-client';

  export default {
      User: {
          fullName: (parent, __, { request }) => {
              // firstName + lastName = FullName
              console.log(parent);
              return `${parent.firstName} ${parent.lastName}`;
          },
          amIFollowing: async (parent, __, { request }) => {
              const { id: parentId } = parent;
              const { user } = request;
              const existing = await prisma.$exists.user({
                  AND: [
                      { id: parentId }, // 팔로잉하려는 사람의 ID가 존재하면서
                      { followers_some: user.id } // 그사람의 팔로워에 내가 있는가?
                  ]
              });
              if (existing) {
                  return true;
              } else {
                  return false;
              }
          },
          itMe: (parent, __, { request }) => {
              //check me
              const { user } = request; //현재 로그인한 유저 정보
              const { id: parentId } = parent; // User를 호출한 Query
              if (parentId === user.id) {
                  return true;
              } else {
                  return false;
              }
          }
      }
  };

  ```
  
  위 코드는 **fullName,itMe**는 동작합니다. 이 코드를 작성할 때 중요한 것이 `parent`라는 것입니다. 평소에 resolver를 작성할 때
  ```
  (_, args, { request, isAuthenticated })
  ```
  이 부분에 대해서 제대로 짚고 넘어가지 않았습니다.  
  첫번째 인자로 사용하지 않는 값, 두번째는 arguments, 세번째는 context로 사용했습니다. 하지만 Computed를 사용할 때는 첫번째 argument를
  사용해야 합니다. 첫번째로 들어오는 argument는 개발자들 사이에서 **root**혹은 **parent**라고 불립니다. 간단하게 얘기 하면 현재 
  Model(?)을 호출한 객체라고 얘기하는 것 같습니다. 아래 코드로 예를 들어보겠습니다.  
  
  ```
  {
	seeUser(id:"cjx7jfr1mpo3w0b121w1bgm6x"){
    
      user{
        firstName
        lastName
        fullName
        amIFollowing
        itMe
      }
    }
  }
  ```
  
  computed에서 생성한 fullName 을 호출하고 있는 user가 있습니다. 이럴 경우 fullName의 첫번째 argument에 들어오는 **parent**는 
  user가 되는 것 입니다. 그렇기 때문에 
  ```
  return `${parent.firstName} ${parent.lastName}`;
  ```
  위와 같이 부모(즉,User)의 fistName 과 lastName을 붙혀주면 FullName 이 되는 것입니다.  
  
　  
   
***

　  
> ### 끝으로 

  amIFollowing 은 왜 동작하지 않는지 모르겠지만 항상 false를 return 해주고 있습니다. 아 그리고 Follower_some과 같은 많은 메소드(?)들에 
  대해서 알고 있으면 손쉽게 API를 작성할 수 있을 것 같습니다. 
  
  
  
