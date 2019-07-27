---
layout: post
title:  "[인스타클론코딩] [Server].26 - prismagram dependency 이슈 해결    "
date:   2019-06-29-21:24:00
author: 한만섭
categories: clonecoding
tags: instagram seeFeed
---



## dependency이슈 

`pakage.json`

```json
{
    "name": "prismagram",
    "version": "1.0.0",
    "description": "Instagram clone with Express + Prisma + React and React Native",
    "main": "index.js",
    "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1",
        "start": "nodemon --exec babel-node src/server.js",
        "deploy": "prisma deploy",
        "generate": "prisma generate",
        "prisma": "npm run deploy && npm run generate"
    },
    "repository": {
        "type": "git",
        "url": "git+https://github.com/13akstjq/prismagram.git"
    },
    "author": "mansub",
    "license": "ISC",
    "bugs": {
        "url": "https://github.com/13akstjq/prismagram/issues"
    },
    "homepage": "https://github.com/13akstjq/prismagram#readme",
    "dependencies": {
        "@babel/cli": "^7.4.4",
        "@babel/core": "^7.4.5",
        "@babel/node": "^7.5.5",
        "@babel/preset-env": "^7.4.5",
        "babel-core": "^7.0.0-bridge.0",
        "dotenv": "^8.0.0",
        "graphql-tools": "^4.0.5",
        "graphql-yoga": "^1.18.0",
        "jsonwebtoken": "^8.5.1",
        "merge-graphql-schemas": "^1.5.8",
        "morgan": "^1.9.1",
        "nodemailer": "^6.2.1",
        "nodemailer-sendgrid-transport": "^0.2.0",
        "passport": "^0.4.0",
        "passport-jwt": "^4.0.0",
        "prisma": "^1.34.0",
        "prisma-client-lib": "^1.34.0"
    },
    "devDependencies": {
        "babel-cli": "^6.26.0",
        "babel-preset-env": "^1.7.0",
        "babel-preset-stage-3": "^6.24.1",
        "nodemon": "^1.19.1"
    }
}

```



![1564198052112](../../../../assets/image/1564198052112.png)

위와 같은 dependency를 가져야 한다.  



> ### 이슈 1. 

`Requires Babel "^7.0.0-0", but was loaded with "6.26.3". `

정확한 이유는 모르겠지만 원래 프로젝트에 설치되어 있던 dependency에 babel버전이 이것저것 설치가 되어있었는데 `babel`과 `@babel`이 같이 설치가 되어있었고, 그것들끼리 충돌이 난 것으로 생각됩니다.  

* 해결방법 

  `babel`들을 다 지우고 `@babel`버전만 설치했더니 해결됐습니다.  

  



> ### 이슈 2. 

`JwtStrategy requires a secret or key`

위 에러는 `javaWebToken`사용시 `.env`파일이 존재하지 않을 경우 발생하는 에러입니다.  

`.env`파일을 `.gitognore`를 통해서 git에 올리지 않았기 때문에 clone을 받았을 때 `.env`파일이 존재하지 않아서 발생했습니다.  

* 해결방법 

  `.env`파일을 만들어주고 설정해주면 됩니다.   