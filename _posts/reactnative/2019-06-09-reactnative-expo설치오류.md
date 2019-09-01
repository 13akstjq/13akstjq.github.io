---
layout: post
title:  "[ReactNative] expo 설치시 react native가 설치되지 않는 이슈  "
date:   2019-06-09 14:16:00
author: 한만섭
categories: reactnative
tags: reactnative expo install issue
---

* TOC
{:toc}


> ## expo 설치시 발생하는 에러 
설치할 파일 경로로 이동 후 
```
expo init "프로젝트 명"
```
expo 프로젝트를 생성했는데 `react native is not install`이라는 메세지를 볼수도 있다. 
정확한 이유는 없지만 다시 설치만 해주면 정상적으로 동작하는 이슈이다. 

> ### 해결 방법
pakage.json에 없는 dependency를 추가해주면 되는데, npm이 깔려있기 때문에 명령어 하나면 쳐주면 된다.  
```
npm i
```
위 명령어는 자동으로 필요한 `dependency`를 `pakage.json`에 설치해주는 명령어이다. 
