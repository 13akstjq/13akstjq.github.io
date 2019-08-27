---
layout: post
title:  "[React] Webpack4 Babel7 React Typescript를 설정하며 느낀 점 "
date:   2019-08-27-20:58:00
author: 한만섭
categories: react
tags: react router Webpack Babel setting
---



[TOC]



## 서론 

이번 포스팅에서는 웹팩, Babel, 리액트,타임스크립트로 구성된 프로젝트 초기 셋팅 과정에서 알게된 내용에 대해서 정리해보려고 합니다. 정확한 내용보다는 제가 이해하기 쉽게 정리하기 때문에 내용의 정확성은 떨어질 수 있습니다.  



***



## 본론



우리가 보고 있는 웹 사이트는 **html**파일들로 구성되어 있습니다. 웹이라는 것이 나온지 오래 됐지만 이것은 변하지 않았습니다. 하지만 사람들은 좀 더 interactive한 site를 보여주고 싶었습니다. 그렇기 때문에 사용했던 것이 **javascript**입니다. JS는 동적인 웹을 사용하기에 적합했습니다.   



**React**는 그런 JS의 프레임워크로써 SPA (Single Page Application) 을 개발하기 위해 사용됐습니다. React로 작성한 코드는 웹이 알아볼 수 없었기 때문에 변환해주는 것이 필요했습니다. 그것이 **Babel**입니다. Babel은 ES6, React와 같은 세련된 코드를 오래된 코드로 변환시켜서 cross browse 이슈를 해결해줄 수 있습니다.   

하지만, JS를 사용하는 웹 개발에도 불편한 점이 있었습니다. C, JAVA와  다르게 타입이 없는 언어이기 때문에 자유롭긴하지만 자유로운만큼 규칙이 없어 개발에 불편함이 있었습니다.  그래서 마이크로 소프트에서 개발한 것이 **TS**(Typescript) 입니다. 타입이 있는 javascript입니다. 이 것은 개발자가 개발하는데 편리함을 위해서 만들어진 언어 입니다. **JS**로 이루어진 Web에게는 전혀 필요 없는 언어입니다. 그래서 우리는 TS를 JS로 변환시켜줄 수 있는 무엇인가가 필요하고 그것이 **TS-Loader**입니다.  



React와 TS를 사용한 많은 tsx파일들은 js파일로 변환되게 되고 html위에 그려지게 됩니다. 하지만 이 부분에서도 문제가 있습니다. 수 많은 JS파일을 요청하게 될경우 병목현상이 발생하게 됩니다. 이런 부분을 해결하기 위해서 사용하는 것이 **Web Pack**입니다.  웹팩은 기준이 되는 entry파일에 import된 모든 js파일들을 하나의 js파일로 묶어주고 그것을 내보내줍니다. 이런 역할을 통해 React에서 사용되는 수 많은 js파일들은 하나의 js로 만들어서 js병목현상을 막을 수 있습니다. 이렇게 웹 프로젝트를 셋팅하는데는 하나하나 다 이유가 있었던 것을 알게 되었습니다.  



## 결론

- Html

  웹에서 그려지기 위한 도화지 역할 

- JS

  interactive한 website를 만들기 위한 웹 언어 

- React

  SPA로 개발하기 위한 javascript Framework

- TypeScript

  타입이 없는 javascript에 type을 추가한 언어

- Babel

  ES6, react와 같은 세련된 코드를 오래된 코드로 변환시켜줌 

- Web Pack 

  index.html에 import되는 수 많은 js 파일을 하나의 번들로 제공해주는 라이브러리 