---
layout: post
title: "[It's me] JobSlider "
date: 2019-09-14-16:47:00
author: 한만섭
categories: portfolio
tags: itsme jobSlider component
---

* TOC
{:toc}






## 1. 기본 프로젝트 셋팅

```bash
mkdir itsme-backend
cd itsme-backend
```



### 1.2 npm init

```bash
npm -y init
```



### 1.3 라이브러리 설치 

```bash
yarn add nodemon -D
yarn add babel/cli
yarn add babel/node 
```

`src/server.js`생성 

```js
console.log("hello")
```



### 1.4  .env 파일 생성 

`src/.env` 생성

```js
PORT = 4000
```



### 1.5 nodemon.json 생성 

루트 디렉토리에 `nodemon.json` 생성해야합니다.  

```
{
  "ext": "js graphql"
}

```



### 1.6 테스트 

`powershell`에서 아래 코드를 실행해보겠습니다.   

```bash
yarn dev
```

![1568547163755](../../../../assets/image/1568547163755.png)



***





## 2. graphql server 셋팅 

```bash
yarn add graphql-yoga
```



`src/server.js`

```js
require("dotenv").config();
import { GraphQLServer } from "graphql-yoga";
const PORT = process.env.PORT || 4000;

//typeDefs
const typeDefs = `
    type Query{
        hello : String!
    }
`;
//resolvers
const resolvers = {
  Query: {
    hello: () => "hello"
  }
};
const server = new GraphQLServer({ typeDefs, resolvers });
server.start({ port: PORT }, () =>
  console.log(`server is running http://localhost:${PORT}`)
);

```



위 상태에서 서버를 실행시켜보겠습니다.   

```bash
yarn dev
```

그러면 아래와 같은 에러가 발생했습니다.   



### 2.1 .babelrc파일 설정 

babel설정이 되어있지 않아서 발생한 에러입니다.  

![1568546481041](../../../../assets/image/1568546481041.png)



루트 디렉토리에 `.babelrc`파일 생성하겠습니다.   

`.babelrc`

```json
{
  "presets": ["@babel/preset-env"]
}
```



```bash
yarn add @babel/node @babel/preset-env
```

위 라이브러리를 설치 한 후에 서버 재실행해보겠습니다.  

```bash
yarn dev
```

아래와 같은 에러 발생했습니다.   

![1568547385441](../../../../assets/image/1568547385441.png)

`@babel/core`찾을 수 없다고 하니 설치해주겠습니다.  

```
yarn add @babel/core
```

설치한 후에 서버 재 실행해보겠습니다.   

```bash
yarn dev
```

아래와 같은 에러가 발생했습니다.   

![1568547483007](../../../../assets/image/1568547483007.png)

보아하니 버전의 문제 인것 같아서 기존에 `babel/cli`를 지워줬습니다.  

```bash
yarn remove babel/cli
```

`@babel/cli`버전으로 재설치하겠습니다.  

```bash
yarn add @babel/cli
```

babel의 7.0버전 부터는 **@**를 붙혀서 사용해야 한다고 합니다.  

이제 진짜 서버 다시 실행해보겠습니다.  

```bash
yarn dev
```

![1568547750073](../../../../assets/image/1568547750073.png)

정상적으로 서버가 실행되는 것을  확인할 수 있습니다.  



### 2.2 Graphql 서버 테스트 

아래 명령어를 통해서 서버 실행해보겠습니다.  

```bash
yarn dev
```

![1568547980655](../../../../assets/image/1568547980655.png)

터미널에서 서버가 정상적으로 실행된 것을 확인했다면 주소로 접속해보겠습니다.  

![1568548049025](../../../../assets/image/1568548049025.png)

위와 같은 화면이 나오면 성공 



![1568548088034](../../../../assets/image/1568548088034.png)

DOCS를 확인해보면 **hello**라는 query를 요청하면 string을 return 해준다는 것을 알 수 있습니다.  

요청을 해보겠습니다.  



![Honeycam 2019-09-15 20-49-24](../../../../assets/image/Honeycam 2019-09-15 20-49-24.gif)

![1568548188888](../../../../assets/image/1568548188888.png)



```js
{
 hello
}
```

위와 같이 작성하게 되면 backend에서 정의했던 **types** 와 **resolver**에 맞춰서 데이터가 응답되는 것입니다. server.js를 살펴보면 아래와 같은 type과 resolver가 있습니다.  

![1568548324090](../../../../assets/image/1568548324090.png)

typeDefs는 query의 Type을 정하는 부분입니다.  

resolvers에서 hello라는 query를 요청하면 string  "hello"를 return 하기로 정해놨기 때문에 graphql playground에서 "hello"가 나온 것입니다.

```js
Query : {
	쿼리 명 : 함수 
}
```



### 2.3 logger 셋팅 

현재 서버의 상태를 log할 수 있는 logger 설치. logger은 express서버에서 사용하는 일종의 미들웨어입니다.  

- installation 

```bash
yarn add morgan
```

- usage

```
import logger from "morgan";
```

`server.express`라고 하면 express서버에 접근할 수 있습니다.   

![1568549075349](../../../../assets/image/1568549075349.png)

```js
server.express.use(logger("dev"))
```



서버 재실행을 해보겠습니다.  

```bash
yarn dev
```



![Honeycam 2019-09-15 21-07-31](../../../../assets/image/Honeycam 2019-09-15 21-07-31.gif)

위와 같이 로그가 계속 나오는 것을 확인했다면 성공적으로 적용된 것입니다.  



### 2.4 schema.js 생성 

`src/api`안에 있는 graphql과 resolver들을 하나로 묶어줄 루트 파일인 schema.js가 필요합니다.  

`src/schema.js` 생성 하나로 묶어줄 때는 몇개의 모듈이 필요함. 설치해보겠습니다.  

```bash
yarn add graphql-tools merge-graphql-schemas
```



```js
// api에 있는 파일들을 여기에서 합침

import path from "path";
import { makeExecutableSchema } from "graphql-tools";
import { fileLoader, mergeResolvers, mergeTypes } from "merge-graphql-schemas";

const allTypes = fileLoader(path.join(__dirname, "/api/**/*.graphql"));
const allResolvers = fileLoader(path.join(__dirname, "/api/**/*.js"));

const schema = makeExecutableSchema({
  typeDefs: mergeTypes(allTypes),
  resolvers: mergeResolvers(allResolvers)
});

export default schema;

```



fileLoader : 특정 형식의 파일을  load해주는 모듈

mergeResolvers : fileLoader로 load한 파일들을 하나의 Resolver로 묶어주는 모듈

mergeTypes: fileLoader로 load한 파일들을 하나의 Types로 묶어주는 모듈

makeExecutableSchema : 묶은 Type 과 Resolver를 합쳐서 하나의 Schema로 만들어주는 모듈



`src/api/Hello/sayHello`에 아래 두 파일 생성 

```js
// sayHello.graphql
type Query {
  sayHello: String!
}

```

```js
// sayHello.js
export default {
  Query: {
    sayHello: () => "Hello"
  }
};

```

`.graphql` : Query의 형식을 정의하는 파일 (ex. sayHello쿼리는 String을 필수적으로 return 함)

`.js` : Query가 실제로 어떤 동작을하는지 작성 (ex. sayHello는 "Hello"를 return 함.)



type과 resolver를 api폴더 안에 만들고 그것을 하나의 schema에 묶었으니 이제 server에서 schema를 사용해보겠습니다.  

`src/server.js`

```js
require("dotenv").config();
import logger from "morgan";
import { GraphQLServer } from "graphql-yoga";
import schema from "./schema";
const PORT = process.env.PORT || 4000;

const server = new GraphQLServer({ schema });
server.express.use(logger("dev"));
server.start({ port: PORT }, () =>
  console.log(`server is running http://localhost:${PORT}`)
);

```

기존에 적혀있던 Types와 resolver를 api안에 넣었기 때문에 굉장히 짧아진 것을 확인할 수 있습니다.  



테스트 해보겠습니다.  



```bash
yarn dev
```

![1568551112361](../../../../assets/image/1568551112361.png)

해당 주소로 접속하면 아래와 같이 playground가 나옵니다.  

![1568551136844](../../../../assets/image/1568551136844.png)

```js
{
	sayHello
}
```

위 코드를 입력하면 "Hello"를 리턴해주는 것을 확인할 수 있습니다. 이렇게 server.js에서 직접 type과 resolver를 설정하지 않고 api폴더 안에 설정해주는 방식까지 적용해보았습니다.  



