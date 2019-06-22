---
layout: post
title:  "[GraphQL] graphQL로 서버만들기(3) "
date:   2019-06-18-22:30:00
author: 한만섭
categories: graphql
tags: GraphQL
---


> ### 정리할 내용 
  이 글에선 **graphql**에서의 swagger같은 playground를 사용하여 데이터를 요청하는 것을 정리해보려고 합니다.  
  
  
> ### query 보내보기 

  localhost:4000 에 들어가면 graphql의 playground를 사용할 수 있습니다. 왼쪽에서 query를 사용하면 됩니다.  
  
  ```
  query{
    name
  }
  ```
  
> ### query 날릴 때 주의할 점 
![image](https://user-images.githubusercontent.com/46010705/59688345-72899380-9218-11e9-8c78-329f03e19419.png)  
위와 같이 person 객체를 요청할 때는 `속성`까지 명시해주어야합니다. (name , age, gender)  

그렇게 할 경우 아래와 같이 데이터를 잘 받아오는 것을 확인할 수 있습니다. 

![image](https://user-images.githubusercontent.com/46010705/59688799-63efac00-9219-11e9-8fb4-4da4bfd98e0b.png)
