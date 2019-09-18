---
layout: post
title:  "[redux] WebPack React Typescript Redux를 이용한 Todo만들기"
date:   2019-08-27 22:05:00
author: 한만섭
categories: redux
tags: redux WebPack react Typescript redux
---





* TOC
{:toc}



## 서론 





## 본론 



### 1. 프로젝트 설정 



#### 프로젝트 생성

```bash
mkdir redux-todo
```



#### 프로젝트로 이동

```bash
cd redux-todo
```



#### npm 셋팅 

```bash
npm init -y
```



`dist/index.html`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
  </head>
  <body>
    <div id="root"></div>
    <script src="/bundle.js"></script>
  </body>
</html>

```





### 2. webpack 설정 



#### webpack 설치 

```bash
npm install --save-dev webpack webpack-dev-server webpack-cli
```



#### pakage.json 설정   

`pakage.json`

```json
{
  "name": "redux-todo",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "start": "webpack-dev-server --config ./webpack.config.js --mode development ",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^4.39.3",
    "webpack-cli": "^3.3.7",
    "webpack-dev-server": "^3.8.0"
  }
}

```



#### webpack 설정 

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



#### 엔트리 코드 작성

`./src/index.js`

```js
console.log("webpack test");
```



#### 실행 

![1566912258396](../../../../assets/image/1566912258396.png)

![1566912285167](../../../../assets/image/1566912285167.png)

위와 같이 웹팩 셋팅 후 실행 되는 것 확인 



<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="4307878116"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>





### 3. React 설치 

```bash
npm i react react-dom
```



### 4. Typescript 설치   



#### 설치 

![1566912897704](../../../../assets/image/1566912897704.png)





#### 설정 

![1566912935783](../../../../assets/image/1566912935783.png)

````json
{
  "compilerOptions": {
    "outDir": "./dist/",
    "noImplicitAny": true,
    "module": "es6",
    "target": "es5",
    "jsx": "react",
    "allowJs": true
  }
}
````



#### 웹팩 설정파일 수정 

`module` 부분과 `resolve`부분을 추가해줍니다. 

```js
module.exports = {
  entry: ["./src/index.tsx"],
  module: {
    rules: [
      {
        test: /\.tsx?$/,
        use: "ts-loader",
        exclude: /node_modules/
      }
    ]
  },
  resolve: {
    extensions: [".tsx", ".ts", ".js"]
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



`src/index.tsx`

```js
import React from "react";
import ReactDOM from "react-dom";

ReactDOM.render(<div>typescript test</div>, document.getElementById("root"));

```

typescript 를 사용할 것이기 때문에 코드를 위와 같이 수정 



<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="4307878116"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>



![1566913399923](../../../../assets/image/1566913399923.png)

기본적인 문법은 허용한다는 설정을 해야합니다.  tsconfig.json을 아래와 같이 수정해줍니다.  

`tsconfig.json`

`"allowSyntheticDefaultImports": true`추가 

```json
{
  "compilerOptions": {
    "outDir": "./dist/",
    "noImplicitAny": true,
    "module": "es6",
    "target": "es5",
    "jsx": "react",
    "allowJs": true,
    "allowSyntheticDefaultImports": true
  }
}

```



`index.tsx`

```js
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";

ReactDOM.render(<App></App>, document.getElementById("root"));

```



`App.tsx`

```js
import React from "react";

const add = (a, b) => a + b;

export default () => {
  return <div>typescript Test</div>;
};

```

![1566913699125](../../../../assets/image/1566913699125.png)

![1566913747810](../../../../assets/image/1566913747810.png)

타입이 적용되지 않을 경우 위와 같은 이슈를 띄어주는 것을 보아 typescript가 정상적으로 setting 된 것 같습니다.  타입을 지정해주면 아래와 같이 정상적으로 동작합니다.  

![1566913797276](../../../../assets/image/1566913797276.png)



ts-loader가 babel-loader의 역할을 해주기 때문에 babel을 일단은 사용하지 않아도 이슈가 없어보임.  





### 5. Redux 설치 

_Redux_는 `@types`버전과 원래버전 둘다 설치를 해야합니다.  

- 원래 버전 설치 

```bash
npm install react-redux redux 
```

- @types 버전 설치 

```bash
npm install @types/react-redux @types/redux
```





## 결론 

위와 같이 프로젝트 설정이 끝났으니 다음 포스팅에서는 Todo를 제작하는 것을 정리하도록 하겠습니다. 

