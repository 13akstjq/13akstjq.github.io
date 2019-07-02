---
layout: post
title:  "[GraphQL] GraphQL로 서버만들기(1) "
date:   2019-06-18-20:31:00
author: 한만섭
categories: graphql
tags: GraphQL
---


> ## GraphQL이란?
  GraphQL은 `over-fetch`와 `under-fetch`를 개선해주는 기능을 갖고 있다.  
  over-fetch : 요청한 데이터보다 서버에서 더 많은 데이터를 보내주는 것  
  under-fetch : 하나의 데이터를 위해 너무 많은 요청을 따로 보내는 것  
  이런 문제점을 개선할 수 있는 것이 `GraphQL`이다.  
  
  　  
  
  ***
  
> ### 스키마는 없는 비어있는 서버 만들기 

  * `mkdir`로 파일 만들기 
  
  ```
  C:\Users\mshan\Desktop\react-workspace>mkdir graphql-test
  ```
  
  
  * `cd` 명령어로 프로젝트 경로로 이동 
  
  ```
  C:\Users\mshan\Desktop\react-workspace>cd graphql-test
  ```

  
  * `npm init`명령어로 npm 설치하기 
  
  ```
  C:\Users\mshan\Desktop\react-workspace\graphql-test>npm init
  ```
  
  * npm을 설치하면 아래와 같은 화면이 나오는데 입력할 부분은 입력하면 된다. 
  
  ```
  package name: (graphql-test)
  version: (1.0.0)
  description:
  entry point: (index.js)
  test command:
  git repository:
  keywords:
  author:
  license: (ISC)
  ```
  
  
  * graphql-yoga 설치하기 
  
  ```
  npm add graphql-yoga
  ```
  
  
  * node-mon 설치하기 
  
  ```
  npm install -g nodemon
  ```
  *pakage.json*파일의 start부분을 추가해줍니다. 
  ```
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start" : "nodemon"
  },
  ```
  *index.js*파일을 만들어 test용 console을 작성합니다. 
  
  위와 같이 설정하고 npm을 시작해줍니다. 
  ```
  npm start
  ```
  
  그러면 아래와 같이 동작하는 것을 확인할 수 있습니다. 
  ```
  PS C:\Users\mshan\Desktop\react-workspace\graphql-test> npm start

> graphql-test@1.0.0 start C:\Users\mshan\Desktop\react-workspace\graphql-test
> nodemon

[nodemon] 1.19.1
[nodemon] to restart at any time, enter `rs`
[nodemon] watching: *.*
[nodemon] starting `node index.js`
hello
[nodemon] clean exit - waiting for changes before restart
  ```
  
앞으로 해야할 작업은 graphql yoga를 가져와야하는데 여기서 옛날 코드를 사용하지 않기 위해서 예쁜(?)코드를 작성하고 babel로 변환시켜줄 겁니다. 

*index.js*에 다음과 같이 코딩합니다. 

```
//const graphqlyoga = require('graphql-yoga'); 구식
import {GraphQLServer} from 'graphql-yoga';

console.log("hello");
```
그러면 `App crash라는 에러가 뜰것입니다. 아주 정상입니다. 왜냐하면 신식 코드를 쓰고 변환해주지 않았기 때문입니다. 

이제 babel-node를 설치해주겠습니다. 

*babel-node 설치하기 

```
npm install --save-dev @babel/node
```

* .babelrc 파일만들기 
```
{
    "presets": ["env","stage-3"]
}
```

* 마무리설치하기 

```
npm install babel-cli babel-preset-env babel-preset-stage-3 --save-dev
```
  
  
* pakage.json 
```
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "nodemon --exec babel-node index.js"
  },
```
  
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
