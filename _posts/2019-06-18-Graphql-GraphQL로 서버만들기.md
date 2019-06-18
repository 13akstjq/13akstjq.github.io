---
layout: post
title:  "[GraphQL] graphQL "
date:   2019-06-18-20:31:00
author: 한만섭
categories: GraphQL
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
  
