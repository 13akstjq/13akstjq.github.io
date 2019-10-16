---
layout: post
title: "Manstagram - server.3 - passport Jwt사용해보기"
date: 2019-10-14-15:23:00
author: 한만섭
categories: manstagram
tags: React ReactHooks ReactNative Graphql Prisma Apollo
---



* TOC
{:toc}
![image](img/66882057-ee97ba80-f003-11e9-98f5-a4bb86148828.png)

사용자 인증을 위해 `passport`라는 인증 모듈을 사용하려고 합니다. Token에 대한 개념과 로그인 인증 방법에 대해서 정리해보려고 합니다.

------

### token 인증 방식

- [x] 1. 인증키가 같은지 확인하기
- [x] 2. 토큰 생성하기 -jwt
- [x] 3. 동일할 경우 토큰 부여하기

***



### passport

이메일로 보낸 인증키와 사용자가 입력한 인증키가 같을 경우 사용자에게 토큰을 리턴해주어야 합니다. 그 때 사용하는 인증 모듈을 `passport`라고 합니다.  
jwt토큰이나 쿠키에서 정보를 가져와서 사용자 정보에 serialize(저장)합니다. 토큰에서 정보를 가져와서(express의) request에 붙혀주는 것입니다.
토큰을 가져와서 해독한 후에 사용자 객체를 request에 추가해줍니다.

#### 1. passport, passport jwt 설치

```bash
npm install passport-jwt passport
```



#### 2. passport.js 작성

사용자 인증을 위한 passport.js 구현 [passport-jwt](https://github.com/mikenicholson/passport-jwt) [jwt](https://www.npmjs.com/package/jsonwebtoken)

```js
// .env 파일을 import하기위한 코드
import dotenv from "dotenv";
import path from "path";
dotenv.config({path : path.resolve(__dirname, ".env")});


import passport from "passport";
import {Strategy , ExtractJwt} from 'passport-jwt';
import { prisma } from "../generated/prisma-client";


const JWTOptions = {
    jwtFromRequest  : ExtractJwt.fromAuthHeaderAsBearerToken(),
    secretOrKey : process.env.JWT_SECRET
}

const verifyUser = (payload, done) =>{
    try{
        const user = prisma.user({id : payload.id});
        if(user !== null){
            return done(null,user);
        }else {
            return done(null,false);
        }
    }catch{
        return done(err,false);
    }
};

passport.use(new Strategy(JWTOptions,verifyUser));
```



#### 3. Token 생성하기

**confirmSecret.js**에서 **generateJwt()**호출

- confirmSecret.js

  ```js
  import { prisma } from "../../../../generated/prisma-client";
  import {generateJwt} from '../../../utils';
  export default {
      Mutation : {
          confirmSecret : async (_,args) =>{
              const {email,secret} = args;
              const user = await prisma.user({email});
              if(secret === user.loginSecret){
                  console.log(user.loginSecret);
                  return generateJwt(user.id);
              }else {
                  throw Error("Wrong secret!!!");
              }
          }
      }
  }
  ```

  

- utils.js

  ```
  export const generateJwt = (id) => jwt.sign({id},process.env.JWT_SECRET);
  ```

![image](img/60023855-9eea5780-96d1-11e9-8ce4-9c85c84b3a66.png)

------

