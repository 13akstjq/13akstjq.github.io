---
layout: post
title:  "[인스타클론코딩] [Server].2 - typeDefs, Resolvers 생성하기 "
date:   2019-06-22-15:30:00
author: 한만섭
categories: clonecoding
tags: graphql babel babel-node dotenv 환경변수
---

* TOC
{:toc}

> ### logger 설치하기 
  * import
  ```
  npm add morgan
  ```
  * .js
  ```
  import logger from 'morgan';
  server.express.use(logger("dev"));
  ```
  * console
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
  
  
  
> ### merge 에 필요한 라이브러리 다운 
  
  ```
  npm add graphql-tools merge-graphql-schemas
  ```
  ```
  import path from 'path';
  import {makeExecutableSchema} from 'graphql-tools';
  import {fileLoader , mergeResolvers,mergeTypes} from 'merge-graphql-schemas';
  ```
  
  * fileLoader : 인자로 path를 받아 파일을 load해줍니다.  
  * path.join : 해당 경로의 파일들을 가져옵니다.  
  * makeExecutabelSchema : type, resolver를 schema 형태로 만들어 줍니다.  
  
> ### 만든 schema를 서버에서 사용하기 
  
  schema를 만들었기 때문에 이제 server에서 schema를 사용할 수 있습니다. 
  
  * import
  ```
  import schema from './schema';
  ```
  
  * #### schema로 server 실행시키기 
  ```
  const server = new GraphQLServer({schema});
  ```
  
