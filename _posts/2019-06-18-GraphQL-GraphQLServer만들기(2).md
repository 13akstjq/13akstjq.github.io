---
layout: post
title:  "[GraphQL] graphQL로 서버만들기(2) "
date:   2019-06-18-21:40:00
author: 한만섭
categories: graphql
tags: GraphQL
---


지금 정리할 내용은 [graphQL로 서버만들기(1)](https://13akstjq.github.io/graphql/2019/06/18/Graphql-GraphQL%EB%A1%9C-%EC%84%9C%EB%B2%84%EB%A7%8C%EB%93%A4%EA%B8%B0.html)
에 이어서 graphQL로 서버를 만드는데 `schema`를 정의하는 내용을 다뤄보려고 합니다.  
　  

***

　  
> ## schema.graphql 파일 만들기 
프로젝트 내에 `schema.graphql이라는 폴더를 만듭니다. 이 파일에서는 사용자가 어떤 정보를 원할지 정의해놓는 곳입니다. 정의를 할 때는 두가지가 있습니다. 
**qeury**와 **mutation**이 있는데 query는 데이터를 요청하는 것, mutation은 데이터를 수정하는 것 
query가 있으면 query를 해결해줄 resolver가 필요하다. 

graphql/schema.graphql
```
type Query {
    name : String! 
}

```
schema만 정의하면 안되고 resolver도 정의해 주어야함.

graphql/resolvers.js
```
const resolvers = {
    Query : {
        name :  () => "mansub"
    }
}

export default resolvers;
```

index.js
```
import {GraphQLServer} from 'graphql-yoga';
import resolvers from './graphql/resolvers';
const server = new GraphQLServer({
    typeDefs : "graphql/schema.graphql",
    resolvers : resolvers
})
```

