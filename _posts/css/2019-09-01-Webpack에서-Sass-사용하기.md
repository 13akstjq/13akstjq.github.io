---
layout: post
title: "[SCSS] Webpack 설정으로 만든 React프로젝트에서 Scss사용하기 "
date: 2019-09-01-13:43:59
author: 한만섭
categories: scss
tags: scss
---



* TOC
{:toc}


## 정리할 내용

*CRA*를 사용해서 만든 프로젝트가 아닌 직접 *Webpack*설정으로 만든 *React*프로젝트에서 *Scss*를 어떻게 설정하는지 정리 해보려고 합니다. ***sass***와 ***scss***가 헷갈릴 수도 있는데, 먼저 나온 것이 sass이고 그것을 추가적으로 보완한 것이 scss입니다. 저는 scss를 사용하는 방법에 대해서 정리해보겠습니다.

### 1. 설치

우선 *npm*을 통해서 설치를 해줍니다.

```bash
npm i --save--dev node-sass style-loader css-loader sass-loader
```

- _**node-scss**_ : *scss*파일을 *css*로 변환해주는 역할을 합니다.
- _**style-loader**_ : webpack 설정에 필요한 모듈입니다.
- _**css-loader**_ : webpack 설정에 필요한 모듈입니다.
- _**sass-loader**_ : webpack 설정에 필요한 모듈입니다.

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

_`pakage.json`_

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
    "ts-loader": "^6.0.4",
    "typescript": "^3.5.3",
    "webpack": "^4.39.3",
    "webpack-cli": "^3.3.7",
    "webpack-dev-server": "^3.8.0"
  },
  "dependencies": {
    "@types/react": "^16.9.2",
    "@types/react-dom": "^16.9.0",
    "css-loader": "^3.2.0",
    "node-sass": "^4.12.0",
    "react": "^16.9.0",
    "react-dom": "^16.9.0",
    "sass-loader": "^8.0.0",
    "style-loader": "^1.0.0"
  }
}
```

위와 같이 설치가 되었다면 웹팩설정을 변경해줘야 합니다.

_`webpack.config.js`_

```js
module.exports = {
  entry: ["./src/index.tsx"],
  module: {
    rules: [
      {
        test: /\.tsx?$/,
        use: "ts-loader",
        exclude: /node_modules/
      },
      {
        test: /\.scss$/,
        use: [
          "style-loader", // creates style nodes from JS strings
          "css-loader", // translates CSS into CommonJS
          "sass-loader" // compiles Sass to CSS, using Node Sass by default
        ],
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

새로운 규칙하나를 추가해주면 됩니다.

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="9095928724"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

### 2. scss 파일 만들기

이제 설정은 끝났으니 *scss*파일을 만들어서 스타일을 적용시켜보도록 하겠습니다.

_`./src/scss/main.scss`_

```css
@import "./vars.scss";
@import "./body.scss";
```

_`./src/scss/vars.scss`_

```css
$font_color: blue;
$font_family: Arial, sans-serif;
$font_size: 16px;
$line_height: percentage(20px / $font_size);
```

_`./src/scss/body.scss`_

```css
body {
  color: $font_color;

  // Property Nesting
  font: {
    size: $font_size;
    family: $font_family;
  }

  line-height: $line_height;
}
```

위와 같이 코드를 작성하고 다시 *`npm start`*를 하게 되면 아래와 같이 글자 색이 파란색으로 적용된 것을 확인할 수 있습니다.

![1567313554776](../../../../assets/image/1567313554776.png)

---

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<!-- 수평 디스플레이 광고 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="4963641784"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

### 주의

_`./src/scss/main.scss`_

```css
@import "./body.scss";
@import "./vars.scss";
```

위와 같이 변수를 import하기 전에 먼저 body.scss를 import 하게 되면 변수를 찾을 수 없다고 에러가 발생합니다.

### 후기

***styled-components***와 같은 CSS-In-Js ( js파일안에 css코드를 집어넣는 방식) 과 다른 느낌이어서 서로의 장단점을 잘 이해하고 필요한 곳에 사용하는 것이 좋을 것 같습니다. 근데 아직은 styled-components가 익숙하다는 느낌이 듭니다.
