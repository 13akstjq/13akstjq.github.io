---
layout: post
title: "[인스타클론코딩] [Server].4 - prisma model만들기"
date: 2019-06-22-19:12:00
author: 한만섭
categories: clonecoding
tags: graphql prisma
---

- TOC
  {:toc}

## datamodel 추가하기

- ### model을 추가할 때 주의사항
  like와 같은 type은 누가 만들었는지같은 정보도 넣어주어야합니다.

[prisma datamodel](https://www.prisma.io/docs/datamodel-and-migrations/datamodel-MYSQL-knul/)

```
type User {
  id: ID! @id
  following: [User!]! @relation(name: "FollowRelation")
  followers: [User!]! @relation(name: "FollowRelation")
}
```

### 주의할 점

- type을 추가할 때 예전에는 아래와 같은 형식으로 써도 추가가 되었다고 합니다. 하지만 업데이트되고 모든 type에 id를 부여해줘야 합니다.

  ```
  type Comment {
  user : User!
  text : String!
  post : Post!
  }
  ```

  id를 추가한 type

  ```
  type Comment {
  id: ID! @id
  user : User!
  text : String!
  post : Post!
  }
  ```

- relation 설정하기

  아래와 같이 from과 to 에 User를 설정하면 relation 을 설정해주어야 한다.

  ```
  type Message {
  id : ID! @id
  text : String!
  from : User!
  to : User!
  room : Room!
  }
  Errors:

   Message
  × The relation field `from` must specify a `@relation` directive: `@relation(name: "MyRelation")`
  × The relation field `to` must specify a `@relation` directive: `@relation(name: "MyRelation")`
  ```

  relation 을 설정해주었을 때

  ```
  from : User! @relation(name : "From")
  to : User! @relation( name: "To")
  ```

---
