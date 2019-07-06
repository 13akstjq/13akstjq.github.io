---
layout: post
title:  "[GraphQL] GraphQL Setting 요약 "
date:   2019-06-19-12:17:00
author: 한만섭
categories: graphql
tags: GraphQL
---


> GraphQL Setting
GraphQL 설치 방법에 대해서 간단하게 정리해보려고 합니다.  

1. 프로젝트 파일 생성  

2. npm init  

```
npm init
```  

3. graphql-yoga add  

```
npm add graphql-yoga
```  

4. install nodemon   

```
npm install -g nodemon
```  

5.pagage.json 추가  

```
 "scripts": {
  "test": "echo \"Error: no test specified\" && exit 1",
  "start" : "nodemon"
},
```

6. graphgl 불러오기   
해당 순서에서 balel을 이용해 변환을 할 것이라면 아래와 같은 추가 설정이 필요합니다.  

* babel-nodel install  
```
npm install --save-dev @babel/node
```

* babel-cli , babel-preset-env , babel-preset-stage-3 install  
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
