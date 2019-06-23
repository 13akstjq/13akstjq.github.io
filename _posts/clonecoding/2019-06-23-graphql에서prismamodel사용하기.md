---
layout: post
title:  "[인스타클론코딩] [Server].5 - graphql에서 prisma model 사용하기  "
date:   2019-06-23-15:01:00
author: 한만섭
categories: clonecoding
tags: graphql prisma
---


> ### 정리할 내용 
  
  이전에 작성했던 코드에서 **datamodel.prisma**에 작성햇던 코드는 아직 graphql에서 인식하지 못하기 때문에 개발자가 직접 같은 코드를 graphql 형식
  으로 만들어야 합니다. 
 ![image](https://user-images.githubusercontent.com/46010705/59972318-f9978c80-95c7-11e9-8272-576234b59fb2.png)


* graphql코드가 위치해야할 곳은 아래 네모쳐진 곳으로 정해놨기 때문에 그곳에 prisma model을 복사해서 넣으면 됩니다.  
![image](https://user-images.githubusercontent.com/46010705/59972343-45e2cc80-95c8-11e9-9f78-6f69b90fb66c.png)


* prisma와 graphql방식의 차이점은 `@`를 인식하지 못한다는 것이기 때문에 기존에 작성했던 **unique, relation**을 제거해주면 됩니다.  
![image](https://user-images.githubusercontent.com/46010705/59972355-7a568880-95c8-11e9-9f4c-60b273cd364c.png)



> ### prisma의 아쉬운점 

  제 생각대로라면 아래 이미지를 실행했을 때 정상적인 결과가 나와야 하지만 실제로는 그렇지 않습니다.  
  ![image](https://user-images.githubusercontent.com/46010705/59972425-dd94ea80-95c9-11e9-89f2-516937ca7bd8.png)
  
  그 이유는 아래와 같이 prisma가 무한 query에 대해 공격 받을 것에 대해 막아놓은 것이라고 합니다. 그래서 사용하는 것이 `$fragment`라는 것입니다. 
  이것에 대해서는 따로 정리를 하도록 하겠습니다. 
  ```
  following{
    id
    user{
      following{
      user{
      }
      }
    }
  }
  ```
