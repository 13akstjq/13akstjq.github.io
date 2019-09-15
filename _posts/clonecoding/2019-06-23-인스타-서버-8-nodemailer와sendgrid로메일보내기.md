---
layout: post
title: "[인스타클론코딩] [Server].8 - 로그인인증 메일보내기(1)"
date: 2019-06-23-19:28:00
author: 한만섭
categories: clonecoding
tags: graphql nodemailer sendgrid
---

- TOC
  {:toc}

## 정리할 내용

이메일로 인증키를 전송해야 하기 때문에 이메일전송하는 모듈인 **nodemailer**에 대해서 정리해보려고 합니다.

---

### nodemailer로 이메일 보내기

- #### 모듈 설치

  ```
  npm add nodemailer
  npm add nodemailer-sendgrid-transport
  ```

- #### import - {}를 하지 않는 것에 주의. ( {}는 언제 쓰이는지 정리 필요 )

  ```
  import nodemailer from 'nodemailer';
  import sgTransport from "nodemailer-sendgrid-transport";
  ```

- #### utils.js

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

---

## 주의할 점

- import시에 {}를 사용하고 안하고 동작에 영향을 주기 때문에 문제가 되었습니다.

  아래와 같이 작성하면 동작하지 않습니다.

  ```
  import {nodemailer} from 'nodemailer';
  import {sgTransport} from "nodemailer-sendgrid-transport";
  ```
