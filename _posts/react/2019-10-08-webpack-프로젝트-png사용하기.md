---
layout: post
title: "[React] webpack 프로젝트에서 png파일 로드하기  "
date: 2019-10-08-11:58:00
author: 한만섭
categories: react
tags: react webpack png
---

- TOC
  
  {:toc}





### webpack 프로젝트에서 png파일 사용하기 

`webpack.config.js`

```json
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
        test: /\.(png|jpg)$/,
        use: ["file-loader"]
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

새로운 rule을 추가해준다.  



`bash`

```bash
yarn add file-loader
```

