---
layout: post
title:  "[JS] vanillaJS 01 - 변수와 데이터 타입 "
date:   2019-06-15-11:48:00
author: 한만섭
categories: js
tags: js var let data 
---


> ## 시작하기 전에
 개발하는데 있어서 자바스크립트를 사용하지 않는 라이브러리나 프레임워크를 찾아보기도 힘든것 같아서.. 다시 가던길을 멈추고 대충 이해하고 넘어갔던 
 Javascript에 대해서 차근차근 정리해보려고 합니다.  
 　  
 
 ***
 
 　  
> ### JS의 변수 
 
* #### let 
  수정이 가능한 변수 

* #### const  
  수정이 불가능한 변수 ( 완전히 불가능하진 않지만 이해하기 쉽게 표현 )  
  **let**이 필요하지 않은 상황이라면 항상 **const**로 변수를 선언하는 것이 좋다.  
  
  ```javascript
  const a = 100;
  a = 4;
  ```
  
  다음과 같이 작성하면 
  
  ```error
  Uncaught TypeError: Assignment to constant variable.
    at index.js:2
  (anonymous) @ index.js:2
  ```
  
* #### var  
  var도 let과 같이 수정이 가능한 변수이다. 너무 자유로운 변수라서 이제 잘 사용하지 않는 변수이다. **(에러를 발생시킬 가능성이 높기 때문?)**  
  
　    
  
***

　  
> ### JS 데이터 타입 

* #### boolean // true,false
* #### String // "mansub"
* #### Number // 1,2,3,4,5
* #### float // 1.2


