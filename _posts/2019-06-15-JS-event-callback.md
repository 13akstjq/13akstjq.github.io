---
layout: post
title:  "[JS] vanillaJS 02 - JS event  "
date:   2019-06-15 13:37:00
author: 한만섭
categories: js
tags: js function event callback
---

event에 달려있는 callback 함수가 언제 호출되는지 알아보려고 한다. 

* #### 이벤트가 발생할 때 함수 호출 
  
  ```javascript
  function handleResize(){
    console.log("resize!!!!");
  }
  window.addEventListener("resize",handleResize);
  ```
  
  위와 같이 코드를 작성하면 화면이 새로고침되어도 console이 찍히지 않고 화면 크기가 조정될 때만 console에 **resize!!!!**가 찍힌다. 
  
* #### 이벤트가 발생하기도 전에 함수 호출 
  
  ```javascript
  function handleResize(){
    console.log("resize!!!!");
  }
  window.addEventListener("resize",handleResize());
  ```
  이렇게 작성하면 화면이 새로고침 되자마자 console에 **resize!!!!**가 찍히는 것을 볼 수 있다.  
  그리고 이렇게 작성하면 callback 형식이 아니라서 화면크기가 바뀌어도 console에 찍히지 않는다. 
