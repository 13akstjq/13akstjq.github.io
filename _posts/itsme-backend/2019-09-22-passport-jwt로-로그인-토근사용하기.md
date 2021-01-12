---
layout: post
title: "[It's me][backend] passport-jwt사용해서 토큰인증방식 사용하기 "
date: 2019-09-22-14:47:00
author: 한만섭
categories: itsme-backend
tags: itsme passport jwt
---

- TOC
  
  {:toc}



## 정리 

현재 로그인이 되었는지 확인할 수 있는 세션, 토큰 등 많은 방식 중에서 토큰을 사용해서 인증하는 방식을 정리하려고 합니다.  

jsonwebtoken과 passport를 사용할건데, jsonwebtoken은 말그대로 암호화된 토큰을 사용하기 위한 것이고 passport는 jwt인증을 쉽게 할 수 있도록 도와주는 역할을 합니다.  

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<!-- n잡 블로그 사각형 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="2552901794"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

우선 passport를 설치해보겠습니다.  

### 1. 설치 

```bash
yarn add passport passport-jwt
```

설치를 했다면 passport를 설정할 파일을 만들어줍니다.  

`src/passport.js`

```js
import passport from "passport";
import { Strategy as JwtStrategy, ExtractJwt } from "passport-jwt";
```

[공식사이트](https://github.com/mikenicholson/passport-jwt)에 아래처럼 나와 있기 때문에 passport-jwt의 Strategy와 ExtractJwt를 사용해줍니다.  

![1569147102968](../../../../assets/image/1569147102968.png)

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<!-- n잡 블로그 사각형 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="2552901794"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

### 2. jwtOptions

그럼 이제 설정을 작성해줘야합니다.  

```js
const jwtOptions = {
  // header에 bearer스키마에 담겨온 토큰 해석할 것
  jwtFromRequest: ExtractJwt.fromAuthHeaderAsBearerToken(),
  // 해당 복호화 방법사용
  secretOrKey: process.env.JWT_SECERT
};
```

첫번째 옵션은 header의 bearerToken을 해석할 것이라는 옵션이고, 두번째는 암호 해독 방식을 알려주는 설정입니다.  



#### 2.1 secretOrKey 설정하기 

암호 해독 방식을 사용하기 위해서는 랜덤키를 사용해야합니다.  

[randomkeygen](https://randomkeygen.com/) 이라는 사이트에서 하나 골라서 사용하시면 됩니다.  



### 3. passport.use()

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<!-- n잡 블로그 사각형 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="2552901794"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

설정을 다했다면 passport를 사용해야 합니다. 

```js
passport.use(new JwtStrategy(jwtOptions, verifyUser));
passport.initialize();
```

두번째 인자로는 verifyUser라는 복호화 성공시 호출할 함수를 적어줍니다. 저는 유저정보를 확인하는 함수가 필요하기 때문에 verifyUser라고 작성했습니다.  



### 4. callback 함수 

```js
const verifyUser = async (payload, done) => {
  console.log("payload", payload);
  try {
    const user = await prisma.user({ id: payload.id });
    // user가 있을 경우
    if (user) {
      return done(null, user);
    } else {
      return done(null, false);
    }
  } catch (error) {
    return done(error, false);
  }
};
```

payload의 id를 가진 user가 존재한다면 프론트엔드에서 요청한 req에 담겨있던 token이 유효한 토큰이라는 것이기 때문에 로그인이 되어있다고 할 수 있습니다. 그래서 `return done(에러 , 유저)`를 상황에 맞게 하면 됩니다.   



전체 코드 

```js
import passport from "passport";
import { Strategy as JwtStrategy, ExtractJwt } from "passport-jwt";
import "./env";
import { prisma } from "../generated/prisma-client";

// passport-jwt인증에 사용할 옵션
const jwtOptions = {
  // header에 bearer스키마에 담겨온 토큰 해석할 것
  jwtFromRequest: ExtractJwt.fromAuthHeaderAsBearerToken(),
  // 해당 복호화 방법사용
  secretOrKey: process.env.JWT_SECERT
};

// 인증 성공시 콜백함수
const verifyUser = async (payload, done) => {
  console.log("payload", payload);
  try {
    const user = await prisma.user({ id: payload.id });
    // user가 있을 경우
    if (user) {
      return done(null, user);
    } else {
      return done(null, false);
    }
  } catch (error) {
    return done(error, false);
  }
};


// jwtOptions를 기반으로한 jwt전략으로 인증하고 성공시  verifyUser호출
passport.use(new JwtStrategy(jwtOptions, verifyUser));
passport.initialize();

```



여기까지 한 것을 요약해보자면, 

1. 프론트엔드에서 Header에 Token을 담아서 request요청을 보냄
2. 서버가 시작하기 전에 passport가 JWT을 해석함. (해석해서 나오는 값은 User테이블의 id)
3. 해당 id값을 가지고 있는 User가 있는지 확인. 
4. 있으면 done(null,user) 리턴해줌. 

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<!-- n잡 블로그 사각형 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="2552901794"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

이제 리턴해준 user를 가지고 서버로 보내고 있던 request객체에 같이 실어주는 부분이 필요합니다.  



### 5. 미들웨어 사용하기 

저는 graphql서버를 만들었기 때문에 express.use를 통해서 서버에 접근할 수 있습니다. 미들웨어를 통해서 JWT토큰을 인증했을 때 request객체에 user정보를 실어서 서버에게 주는 부분을 작성해보겠습니다.  



`src/server.js`

```js
import logger from "morgan";
import { GraphQLServer } from "graphql-yoga";
import schema from "./schema";
import passport from "passport";
import "./passport";
import { authenticateJWT } from "./passport";
import "./env";
const PORT = process.env.PORT || 4000;

const server = new GraphQLServer({
  schema,
  context: ({ request }) => ({ request })
});
server.express.use(logger("dev"));
server.express.use(authenticateJWT);
server.start({ port: PORT }, () =>
  console.log(`server is running http://localhost:${PORT}`)
);

```

위처럼 AuthenticateJWT라는 JWT인증 함수를 서버가 켜지기 전에 호출했습니다. 그리고 `./passport`를 import함으로써 passport.use()와 passport.initialize()를 실행시킬 수 있습니다.  



그러면 authenticateJWT를 어떻게 작성했는지 보겠습니다.  

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<!-- n잡 블로그 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1235773082"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

![1569136864755](../../../../assets/image/1569136864755.png)





`src/passport.js`

```js
// verifyUser가 호출되고난 후 passportdlswmd
export const authenticateJWT = (req, res, next) =>
  passport.authenticate("jwt", { sessions: false }, (error, user) => {
    //verifyUser에서 user를 찾았다면 서버에게 요청하는 req객체의 user에 담아서 서버에게 넘겨줌
    if (user) {
      req.user = user;
    }
    next();
  })(req, res, next);

```

미들웨어이기 때문에 req,res,next를 파라메터로 받습니다. passport의 authenticate라는 메소드는 

첫번째로 어떤 인증을 할 것인지 적어줍니다. 저는 jwt를 인증을 할 것이기 때문에 "jwt"를 적었습니다.  

두번째는 session은 사용하지 않는다고 적었습니다.  

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<!-- n잡 블로그 사각형 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="2552901794"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

세번쨰는 verifyUser를 통해서 return 된 error와 user를 파라메터로 하는 함수를 작서하는 부분입니다. 저는 user가 있을 경우에 request객체에 user라는 키값으로 데이터를 넣어서 보내야 하기 때문에 위와 같이 user가 있을 경우 미들웨어를 통해 접근할 수 있던 req객체의 user에 해당 user를 집어넣었습니다.  



위와 같이 작성하게 될 경우, 프론트엔드에서 보낸 JWT토큰을 해석해서 user정보를 확인했는데 존재하는 user라면 서버에게 보내는 req객체에 user정보를 넣어서 보낼 수 있습니다. 이렇게 하게 되면 로그인이 필요한 기능에 대해서 쉽게 접근을 제어할 수 있습니다.  



playground에서 header에 대한 부분을 실험하기 위해서는 아래와 같이 사용해야합니다.  

![1569153412440](../../../../assets/image/1569153412440.png)





### 6. 토큰 발급받기 



위의 과정은 토큰을 프론트엔드에서 붙혀서 보냈을 경우 어떻게 처리하는지에 대해서 정리한 것입니다. 하지만 사용자가 로그인에 성공했을 경우 최초 토큰은 서버에서 만들어서 사용자에게 보내주어야합니다.   

[jsonwebtoken](https://www.npmjs.com/package/jsonwebtoken)을 만들기 위해 설치를 하겠습니다.   

```bash
yarn add jsonwebtoken
```

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<!-- n잡 블로그 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1235773082"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

![1569133510842](../../../../assets/image/1569133510842.png)

```js
jwt.sign("token으로 바꿀 데이터" , "암호화 방법")
```

저는 아래와 같이 작성했습니다.  

```js
import jwt from "jsonwebtoken";
import "./env";

// id를 받아서 암호화된 JWTToken을 만들어주는 메소드
export const generateJWTToken = id => jwt.sign({ id }, process.env.JWT_SECERT);
```

저는 .env파일을 만들어서 env 설정을 매번 해주지 않도록 작성했습니다.  



`./env`

```js
import path from "path";
require("dotenv").config({ path: path.resolve(__dirname, ".env") });
```



이렇게 작성하면 이제 로그인에 성공했을 경우 사용자에게 token을 발급해주게 됩니다.  

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<!-- n잡 블로그 사각형 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="2552901794"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

### 7. 인증하기 

이제 개발한 것을 통해 실제로 사용해보겠습니다. allUser Query를 사용하기 위해서는 로그인이 되어있어야 한다고 가정해보겠습니다.  



`src/middleware.js`

```js
// request에 user정보가 없으면 error를 return
// 로그인이 필요한 query , mutation에서 사용하면 됨.
export const isAuthenticate = request => {
  console.log(request.user);
  if (!request.user) {
    throw Error("You need to login for this action!!!");
  }
};

```



`src/api/User/allUser/allUser.js`

```js
import { prisma } from "../../../../generated/prisma-client";
import { isAuthenticate } from "../../../middleware";

export default {
  Query: {
    allUser: async (_, args, { request }) => {
      isAuthenticate(request);
      return await prisma.users();
    }
  }
};

```

query의 첫번째 파라메터는 사용하지 않고, 두번째 파라메터는 사용자가 보낸 객체가 담겨 있고, 세번째는 req객체가 담겨있습니다.  request객체에 user가 담겨있는지 확인하기 위해 저는 isAuthenticate라는 메소드를 만들어서 확인했습니다.  



만일 로그인이 잘 되어있다면 request객체에 아래와 같이 User객체가 담겨서 올 것입니다.  

![1569154067902](../../../../assets/image/1569154067902.png)



만약 토큰을 정상적으로 전달하지 않았다면 아래와 같이 에러메세지를 띄울 것입니다.  

![1569154106650](../../../../assets/image/1569154106650.png)





### 8. 요약 

1. 사용자가 로그인 요청 보냄 

2. 로그인이 성공하면 사용자의 id로 만든 TOKEN을 리턴해줌 

3. 로그인된 사용자는 받은 TOKEN을 항상 request객체에 넣어서 요청함.  

4. 서버에서는 서버를 키기 전에 passport를 통해서 TOKEN이 원래 무슨 ID였는지 복호화 

5. 복호화한 ID를 가진 User가 존재한다면 request객체에 해당 User정보를 넣어서 서버에게 전달함.  

6. 서버는 request 객체에 담긴 user에 따라서 사용자에게 권한을 줄 수 있음. (프로필 보기는 로그인된 회원만 )

   