---
layout: post
title:  "[node] express서버와 mysql 시퀄라이징(sequelizing)으로 연동하기"
date:   2020-03-02 22:28:59
author: 한만섭
categories: nodejs
tags:	nodejs express mysql
---

* TOC
{:toc}
sequelize라는 ORM을 사용해서 mysql을 연동하는 방법입니다. 



### 프로젝트 생성 

```bash
express sequelize-study
cd sequelize-study
npm i 
```





### sequelize 설치 

```bash
npm i sequelize mysql2
```

`sequelize`명령어를 사용하기 위해 글로벌로도 설치해줍니다. 

 ```bash
sudo npm i -g sequelize-cli
 ```

초기화를 해줍니다. 

```bash
sequelize init
```

그러면 아래와 같이 폴더가 생성됩니다. 

![image](https://user-images.githubusercontent.com/46010705/76703877-5fa8de80-6718-11ea-86d3-35349b047793.png)



### mysql 설치 

[mysql 홈페이지](https://dev.mysql.com/downloads/mysql/)에서 해당 OS에 맞는 mysql을 설치합니다. 저는 버전은 5.7.29로 설치하였습니다. 

![image](https://user-images.githubusercontent.com/46010705/76728512-3e88d200-679a-11ea-9353-59cc598df8b6.png)

mysql을 클릭으로 쉽게 다룰 수 있게 도와주는 **workbench**도 다운로드하겠습니다. [사이트](https://dev.mysql.com/downloads/workbench/)

- Mysql 초기비밀번호는 복사해두기 

- workbench실행시키기 전에 mysql 서버 실행시키기 

  ![image](https://user-images.githubusercontent.com/46010705/76729301-2e71f200-679c-11ea-99de-15471bc4876e.png)



### sequelize 설정 

**config.json 수정**

`Config.json`에는 데이터베이스 설정과 비밀번호를 정의할 수 있는 설정파일입니다. 

```json
{
  "development": {
    "username": "root",
    "password": null, // 비말번호 
    "database": "nodebird", // db이름 
    "host": "127.0.0.1",
    "dialect": "mysql",
    "operatorsAliases": false
  }
  ...생략
```



### DB 생성하기 

아래 명령어를 통해서 `config.json`에 설정된 db를 만들 수 있습니다. 

```bash
$ sequelize db:create

Sequelize CLI [Node: 11.15.0, CLI: 5.5.1, ORM: 5.21.5]

Loaded configuration file "config/config.json".
Using environment "development".
(node:56911) [SEQUELIZE0004] DeprecationWarning: A boolean value was passed to options.operatorsAliases. This is a no-op with v5 and should be removed.
Database nodebird created.
```



### nodemon 설치 

서버를 실행시키기 위해서는 원래 `ctrl+c`를 눌러서 종료시킨 후에 다시 실행시켰는데, 서버 코드가 수정될 때마다 감지해서 서버를 다시 실행시켜주는 생상성을 높혀주는 모듈입니다. 

```bash
npm i -D nodemon
```



### express에 필요한 모듈 설치 

```bash
npm i express cookie-parser express-session morgan connect-flash pug
```

`body-parser`는 이제 express에서 내부적으로 지원해주기 때문에 더이상 사용할 필요가 없습니다.



### 서버 코드 작성 

이번에는 `express-generator`를 사용하지 않고 직접 코드를 작성해보겠습니다. 

`app.js`를 만들어줍니다. 

- 설치한 모듈 불러오기 
- `view-engine`인 `pug`설정
- `port`설정
- `morgan`사용 (사용자 요청 로그 확인용)
-  static파일(css,js)에 대한 응답 미들웨어 
- `Body-parser`대신에 사용할 코드 
-  cookie파싱용 미들웨어 사용
-  session사용 
- flash사용 
- 서버 listen

 ```js
const express = require("express");
const cookieParser = require("cookie-parser");
const morgan = require("morgan");
const require = require("path");
const session = require("express-session");
const flash = require("flash");

const app = express();

// view engine
app.set("view-engine", "pug");
app.set("views", path.join(__dirname, "views"));

// port
app.set("port", process.env.PORT || 8001);

// middleware
app.use(morgan("dev"));
app.use(express.static(path.join(__dirname, "public")));
app.use(express.json());
app.use(express.urlencoded({ extend: false }));
app.use(cookieParser("nodebirdsecret"));
app.use(
  session({
    resave: false,
    saveUninitialized: false,
    secret: "nodebirdsecret",
    cooki: {
      httpOnly: true,
      secure: false
    }
  })
);
app.use(flash());

app.listen(app.get("port"), () => {
  console.log(`${app.get("port")}번 포트에서 서버 실행중입니다.`);
});

 ```

static파일들이 들어있을 public과 pug파일이 들어갈  views폴더를 루트 디렉토리에 만들어줍니다. 

### dotenv 모듈 

서버에서 사용하는 중요한 정보들을 `dotenv`모듈을 통해서 관리할 수 있습니다. 

**설치**

```bash
npm i dotenv
```

**사용**

`.env`

```bash
COOKIE_SECRET=nodebirdsecret
PORT=8001
```

`app.js`

```js
require("dotenv").config();

// port
app.set("port", process.env.PORT || 8001);

app.use(cookieParser(process.env.COOKIE_SECRET));
app.use(
  session({
    resave: false,
    saveUninitialized: false,
    secret: process.env.COOKIE_SECRET,
    cooki: {
      httpOnly: true,
      secure: false
    }
  })
);
```





### router 설정 

`routes`폴더 안에 원하는 라우터 파일을 만들어줍니다. 

**라우터 기본 구조 **

```js
const express = require("express");
const router = express.Router();

module.exports = router;
```

`routes/page.js`로 라우터를 만들겠습니다.  page는 서비스의 페이지들을 모아놓는 곳입니다. 

```js
const express = require("express");
const router = express.Router();

// 프로필 페이지
router.get("/profile", (req, res, next) => {
  res.render("profile", { title: "내 정보 | nodebird", user: null });
});

// 회원가입 페이지
router.get("/join", (req, res, next) => {
  res.render("join", {
    title: "회원가입 | nodebird",
    user: null,
    joinError: req.flash("joinError")
  });
});

// 메인 페이지
router.get("/", (req, res, next) => {
  res.render("main", {
    title: "nodebird",
    twits: []
  });
});
module.exports = router;

```

`app.js`에서 불러와 사용합니다. 404,500에 대한 에러처리도 함께 해줍니다. 

```js
const indexRouter = require("./routes/page");
app.use("/", indexRouter);

// 404 not found
app.use((req, res, next) => {
  const err = new Error("not Found");
  err.status = 404;
  next(err);
});

// 500 error
app.use((err, req, res) => {
  res.locals.message = err.message;
  res.locals.error = req.app.get("env") === "development" ? err : {};
  res.status(err.status || 500);
  res.render("error");
});
```

