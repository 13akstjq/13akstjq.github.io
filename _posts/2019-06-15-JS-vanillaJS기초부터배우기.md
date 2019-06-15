---
layout: post
title:  "[JS] vanillaJS 01 - 변수와 데이터 타입 "
date:   2019-06-15 11:48:00
author: 한만섭
categories: js
tags: js var let data 
---


> ## 시작하기 전에
 개발하는데 있어서 자바스크립트를 사용하지 않는 라이브러리나 프레임워크를 찾아보기도 힘든것 같아서.. 다시 가던길을 멈추고 대충 이해하고 넘어갔던 
 Javascript에 대해서 차근차근 정리해보려고 합니다.  
 　  
 
 ***
 
 　  
> ## JS의 데이터 타입 
 
* ####let 
  수정이 가능한 변수 

* ####const  
  수정이 불가능한 변수 ( 완전히 불가능하진 않지만 이해하기 쉽게 표현 )
  
  ```javascript
  const a = 100;
  a = 4;
  ```
  
  다음과 같이 작성하면 
  
  ```console
  Uncaught TypeError: Assignment to constant variable.
    at index.js:2
  (anonymous) @ index.js:2
  ```
