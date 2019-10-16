---
layout: post
title: "Manstagram - server.2 - nodeMailer로 로그인 인증 이메일보내기"
date: 2019-10-14-15:23:00
author: 한만섭
categories: manstagram
tags: graphqlserver nodemailer
---



* TOC
{:toc}


![image](img/66882445-69150a00-f005-11e9-8bbe-ed5f08a8b152.png)

이메일로 인증키를 전송해야 하기 때문에 이메일전송하는 모듈인 **nodemailer**에 대해서 정리해보려고 합니다.

------



#### nodemailer npm install 

```bash
npm add nodemailer
npm add nodemailer-sendgrid-transport
```

```
import nodemailer from 'nodemailer';
import sgTransport from "nodemailer-sendgrid-transport";
```

***



#### utils.js 작성

1. server.js`에서 **sendSecretMail**을 호출 하게 됩니다.
2. arguments로 받은 이메일 주소와 secret으로 **email** 객체를 만들고 `sendMail(email)` 호출합니다.
3. 메일을 보내기 위해서는 transport 객체가 필요합니다. transport객체를 만들기 위해 sendgrid의 api를 이용합니다. 이 과정에서 `.env`파일의
   환경변수를 호출해야하는데 여기서도 .env를 호출할 수 있게 해주는 코드를 선언해야합니다.
4. 만든 transport객체는 client가 갖고 있습니다. 메일을 보내주는 `sendMail(email)`을 호출하면 메일이 보내집니다.

```
// .env 파일을 import하기위한 코드
import dotenv from "dotenv";
import path from "path";
dotenv.config({path : path.resolve(__dirname, ".env")});
```



```js
const sendMail = (email) => {
const options = {
auth: {
api_user: process.env.SENDGRID_USERNAME,
api_key: process.env.SENDGRID_PASSWORD
}
};
const client = nodemailer.createTransport(sgTransport(options));
return client.sendMail(email);
};

export const sendSecretMail = (address,secret) => {
const email = {
from : "mshan7@prismagram.com",
to : address,
subject : "confirm for login",
html : `secret key is ${secret}`
};
return sendMail(email);
}
```

