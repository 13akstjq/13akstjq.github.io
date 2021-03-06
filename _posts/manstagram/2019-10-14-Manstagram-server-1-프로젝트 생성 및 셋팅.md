---
layout: post
title: "Manstagram - server.1 - 프로젝트 생성 및 셋팅"
date: 2019-10-14-15:23:00
author: 한만섭
categories: manstagram
tags: React ReactHooks ReactNative Graphql Prisma Apollo
---



* TOC
{:toc}
![image](https://user-images.githubusercontent.com/46010705/66895178-1c452980-f02d-11e9-9a0a-eaab7ad03dde.png)

### 1 .프로젝트 셋팅

#### 프로젝트 생성

- Git Repository 만들기
- 프로젝트 만들기 **.gitignore : Node**
- npm init 및 Git 연동



#### 라이브러리 설치

- graphql-yoga

  ```bash
  npm add graphql-yoga
  ```

- nodemon -D ** src/server.js 파일 실행시키는데 필요함. **

  ```bash
  npm add nodemon -D
  ```

- babel-node

  ```bash
  npm install --save-dev @babel/node
  ```





#### pakage.json 수정

- **npm dev**를 실행하면 nodemon --exec babel-node src/server.js를 실행하게 되는 것 입니다.
- **nodemon**을 실행할 때마다 babel-node로 src폴더의 server.js 파일을 실행하게 됩니다.
- **nodemon**은 저장할 때마다 실행을 해주는 도구입니다. 서버를 껐다가 킬 필요가 없어지게 해주는 도구입니다.

```json
"scripts" : {
  "dev" : "nodemon --exec babel-node src/server.js"
}
```



#### 프로젝트 환경변수 설정하기 - dotenv

프로젝트를 만들다보면 **PORT**와 같은 변수들을 지정해주어야 할 상황이 있습니다. 원래 이런 것들은 소스코드에 넣는 것이 아니라고 합니다.  
하지만 **.env**파일을 통해서 넣어 놓는 경우가 많다고 합니다.

그래서 **.env**파일을 만들고 **dot.env** 모듈을 이용해 환경변수를 설정해보려고 합니다.

- ##### .env 파일 만들기

  제가 사용할 변수는 포트번호이기 때문에 PORT를 선언해놓으려고 합니다.

  ```json
  PORT = 4000
  ```


- server.js 에서 환경변수 사용하기

  config()에 아무 것도 넣지 않을경우 .env파일로 인식하는 것 같습니다.  
  그리고 **PORT** 변수를 만들어 **process.env.변수명**으로 사용해주면 됩니다.

  ```js
  require("dotenv").config()
  const PORT = process.env.PORT || 4000;
  ```



#### typeDefs, Resolvers 만들기

서버를 만들기 위해서는 query를 생성해야하고 query를 생성하기 위해서는 **Type**과 **Resolvers**가 필요합니다.  
그 Type과 Resolvers를 Server.js안에 우선 만들어보도록 하겠습니다.

- Type은 styled Components만드는 방식
- Resolvers는 평볌한 변수 만드는 방식 - query(조회) 와 mutation(추가,수정,삭제)이 있습니다.
- 그다음에 server를 만들고 typeDefs와 Resolvers를 넣어주고 start해주면 됩니다.

```js
js//typeDefs
const typeDefs = `
    type Query{
        hello : String!
    }
`
//resolvers
const resolvers = {
    Query : {
        hello : () => "hello"
    }
}
const server = new GraphQLServer({typeDefs,resolvers});
server.start({port : PORT}, () => console.log(`server is running http://localhost:${PORT}`));
```

- 결과

```bash
[nodemon] restarting due to changes...
[nodemon] starting `babel-node src/server.js`
[nodemon] restarting due to changes...
[nodemon] starting `babel-node src/server.js`
server is running http://localhost:4000
```

***

### 2. dotenv

[dotenv 참고사이트](https://blog.seq.kr/2018/11/20/nodejs/dotenv-load-enviroment-file/)



***



### 3. logger 설치하기

- import

```
npm add morgan
```

- .js

```
import logger from 'morgan';
server.express.use(logger("dev"));
```

- console

```
POST / 200 40.776 ms - 15748
POST / 200 13.309 ms - 15748
POST / 200 20.666 ms - 15748
POST / 200 19.683 ms - 15748
POST / 200 11.616 ms - 15748
POST / 200 12.903 ms - 15748
POST / 200 18.416 ms - 15748
POST / 200 8.035 ms - 15747
POST / 200 7.729 ms - 15747
```

#### merge 에 필요한 라이브러리 다운

```bash
npm add graphql-tools merge-graphql-schemas
```

```js
import path from 'path';
import {makeExecutableSchema} from 'graphql-tools';
import {fileLoader , mergeResolvers,mergeTypes} from 'merge-graphql-schemas';
```

- fileLoader : 인자로 path를 받아 파일을 load해줍니다.
- path.join : 해당 경로의 파일들을 가져옵니다.
- makeExecutabelSchema : type, resolver를 schema 형태로 만들어 줍니다.



#### gqphqlServer에 schema 사용하기

schema를 만들었기 때문에 이제 server에서 schema를 사용할 수 있습니다.

- import

```js
import schema from './schema';
```

- 실행

```js
const server = new GraphQLServer({schema});
```

