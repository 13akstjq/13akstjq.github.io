---
layout: post
title:  "[Nodejs] Babel 버전에러 해결방법 "
date:   2019-06-21-21:54:00
author: 한만섭
categories: nodejs
tags: nodejs
---

* TOC
{:toc}




* Babel 버전에러 발생시 해결방법
```
Requires Babel "^7.0.0-0", but was loaded with "6.26.3". If you are sure you have a compatible version of @babel/core
```

```
npm install --save-dev "babel-core@^7.0.0-bridge.0"
```
