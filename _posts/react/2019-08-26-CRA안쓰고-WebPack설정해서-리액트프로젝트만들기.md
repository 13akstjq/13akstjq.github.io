---
layout: post
title:  "[React] Webpack4 & Babel7 & React & Node 프로젝트 셋팅하기 "
date:   2019-08-27-00:03:00
author: 한만섭
categories: react
tags: react router Webpack Babel setting
---



[TOC]



## 서론 

기존에 계속 **CRA(create-react-app)**으로 리액트 프로젝트를 개발했었는데 직접 **Webpack**으로 프로젝트를 설정해보는 것이 도움이 될 것이라는 얘기를 듣기도 했고, 앞으로 있을 프로젝트를 만들 때 적용해보기 위해 WebPack & Babel 로 리액트 프로젝트 셋팅하는 것을 정리해보려고 합니다.  



***



## 1. 프로젝트 생성 



### 1-1. 디렉토리 생성 

기존의 CRA를 사용하지 않기 때문에 프로젝트 폴더 부터 생성해줘야 합니다. 저는 프로젝트 명을 **react-webpack-study**라고 만들도록 하겠습니다.  

```bash
mkdir react-webpack-study
```

프로젝트 파일을 만든 후에 생성한 폴더로 들어가도록 하겠습니다.  

```bash
cd react-webpack-study
```

이렇게 하면 폴더를 만들고 그 위치로 들어오는 것 까지 한 것입니다.  



### 1-2. pakage.json 생성  

비어있는 프로젝트 폴더이기 때문에 아래 명령어를 작성하게 되면 **pakage.json**을 생성할 수 있습니다.  `-y`는 기본 값으로 셋팅하겠다는 설정입니다.    

```bash
npm init -y
```

`결과`

![1566834106155](../../../../assets/image/1566834106155.png)

그렇다면 이제 vscode를 열어서 본격적인 셋팅을 하도록 하겠습니다.  



### 1-3.  index.html 작성 

리액트 프로젝트는 SPA로써 하나의 html 을 보여주는 방식이기 때문에 html 파일을 만들어줍니다.  

`dist/index.html`  

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>React Webpack Babel Setup</title>
  </head>
  <body>
    <div id="root"></div>
    <script src="/bundle.js"></script>
  </body>
</html>

```

script는 js파일들을 하나의 Bundle로 만들고난 후에 사용할 것이기 때문에 여기서는 설명하지 않고 넘어가겠습니다.  



## 2. Webpack 셋팅



### 2-1. webpack 설치  



기본 프로젝트 설정이 다 끝났다면 웹팩을 설치해야합니다.  

```bash
npm install --save-dev webpack webpack-dev-server webpack-cli
```

설치가 끝나면 pakage.json이 아래와 같이 적혀있을 것입니다.  

`pakage.json`  

```json
{
  "name": "react-webpack-study",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^4.39.2",
    "webpack-cli": "^3.3.7",
    "webpack-dev-server": "^3.8.0"
  }
}

```





### 2-2. webpack 설정추가



`npm start` 명령어를 통해서 webpack 프로젝트를 실행시킬 수 있도록 하기 위해 pakage.json의 scripts코드를 수정해야합니다.  

```json
{
  "name": "react-webpack-study",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "start": "webpack-dev-server --config ./webpack.config.js --mode development",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^4.39.2",
    "webpack-cli": "^3.3.7",
    "webpack-dev-server": "^3.8.0"
  }
}

```



웹팩의 설정 파일을 추가해줍니다.   

`webpack.config.js`

```js
module.exports = {
  entry: ["./src/index.js"],
  output: {
    path: __dirname + "/dist",
    publicPath: "/",
    filename: "bundle.js"
  },
  devServer: {
    contentBase: "./dist"
  }
};

```





위 설정이 끝났다면,  webpack이 번들 파일을 만들어줄 수 있도록 entry파일을 만들어줘야 합니다.  

위의 웹팩 설정 파일을 보면 `src/index.js`를 entry로 하기로 설정했으니 해당 경로에 파일을 만들어줍니다.  

`./src/index.js`

```js
console.log("webpack setting")
```



`npm start` 명령어를 통해서 실행을 해봅니다.   

![1566835209175](../../../../assets/image/1566835209175.png)

성공했다고 하니 크롬을 켜서 접속해봅니다.  

![1566835269361](../../../../assets/image/1566835269361.png)



위와 같이 console로 작성한 내용이 잘 나온다면 여기까지는 성공한 것입니다~~   

이제 웹팩 설정은 마쳤기 때문에 Babel을 설치해보도록 하겠습니다.  



## 3. Babel 설정 

babel은 간단하게 말해서 세련된 코드를 오래된 코드로 변환시켜주는 것이라고 생각하면 쉽습니다.  

현재 웹 개발에는 ES6같은 세련된 문법들이 많이 사용되고 있는데, 이것은 cross-browsing에 있어서 효과적이지는 않습니다. 그렇게 때문에 모든 웹에서 동작할 수 있는 오래된 코드로 변환시켜주는 부분이 필요합니다. 그것을 Babel이 대신해주는 것입니다. 현재 나와있는 Babel 7버전으로 설정을 해보려고합니다.   



### 3-1. Babel 설치 

```bash
npm install --save-dev @babel/core babel-loader @babel/preset-env @babel/preset-stage-2 @babel/preset-react
```

- @babel/core : babel 기본 
- babel-loader : 컴파일러?
- @babel/preset-env : babel 환경 설정 
- @babel/preset-stage-2 : ES6를 바꿔줌
- @babel/preset-react : React 코드를 js로 변환?  

정확하지는 않지만 위와 같은 느낌으로 사용하는 것 같습니다.  

`pakage.json`

```json
{
  "name": "react-webpack-study",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "start": "webpack-dev-server --config ./webpack.config.js --mode development",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "@babel/core": "^7.5.5",
    "@babel/preset-env": "^7.5.5",
    "@babel/preset-react": "^7.0.0",
    "@babel/preset-stage-2": "^7.0.0",
    "babel-loader": "^8.0.6",
    "webpack": "^4.39.2",
    "webpack-cli": "^3.3.7",
    "webpack-dev-server": "^3.8.0"
  }
}

```



### 3-2. .babelrc 파일 생성 

babel도 설정이 필요하기 때문에 설정 파일을 만들어줍니다.  

`.babelrc`

```
{
  "presets": [
    "@babel/preset-env",
    "@babel/preset-react",
    "@babel/preset-stage-2"
  ]
}
```



### 3-3. webpack.config.js 코드 추가 

```js
 module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: /node_modules/,
        use: ['babel-loader']
      }
    ]
  },
  resolve: {
    extensions: ['*', '.js', '.jsx']
  },
 

```

위 코드를 `webpack.config.js`에 추가해줍니다.  

```js
module.exports = {
  entry: ["./src/index.js"],
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: /node_modules/,
        use: ["babel-loader"]
      }
    ]
  },
  resolve: {
    extensions: ["*", ".js", ".jsx"]
  },
  output: {
    path: __dirname + "/dist",
    publicPath: "/",
    filename: "bundle.js"
  },
  devServer: {
    contentBase: "./dist"
  }
};

```



## 4. React 설치 

Webpack 과 Babel 의 설치 및 설정을 마쳤기 때문에 이제 React를 설치하겠습니다. React프로젝트를 구성하기 위해서는 최소한 설치해야하는 것이 두개 있습니다.  

```bash
npm install --save react react-dom
```

react-dom은 react의 dom에 render를 하기 위해서 사용됩니다.  



react를 설치했다면 기존에 만들었던 `index.js`를 아래와 같이 변경해주도록 하겠습니다.   

`./src/index.js`

```js
import React from 'react';
import ReactDOM from 'react-dom';
 
const title = 'React Webpack Babel Setup';
 
ReactDOM.render(
  <div>{title}</div>,
  document.getElementById('root')
);
```



## 5. 실행 및 디버깅 

이제 `npm start`를 통해서 프로젝트를 시작해보도록 하겠습니다.  

![1566836413213](../../../../assets/image/1566836413213.png)

위와 같은 에러를 발생시키는데 이 것은 Babel7버전 부터 .babelrc 설정할 때 ES6코드를 변환하는 stage를 더이상 하지 않는다고 합니다. 그러면 `.babelrc`파일을 수정하고 다시 `npm start`를 해보겠습니다.  

```json
{
  "presets": [
    "@babel/preset-env",
    "@babel/preset-react",
  ]
}

```

![1566836537279](../../../../assets/image/1566836537279.png)

stage코드를 삭제해주니 정상적으로 동작하는 것을 확인할 수 있습니다.  

![1566836571674](../../../../assets/image/1566836571674.png)



url로 들어가보면 화면도 정상적으로 나오고 react-dev-tools 도 동작하는 것으로 보아 문제 없이 설정된 것 같습니다.  



## 후기 

[참고사이트](https://pro-self-studier.tistory.com/19)를 기반으로 제작했지만 babel6버전으로 제작하여 버전이슈가 있었는데, babel의 경우 7버전부터 `@`를 붙혀주면 사용이 가능했습니다.  

