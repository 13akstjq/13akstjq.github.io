---
layout: post
title:  "[JS] vanillaJS 02 - JS function  "
date:   2019-06-15 12:13:00
author: 한만섭
categories: js
tags: js function DOM
---


* ### DOM에 있는 function 을 사용해서 객체를 가져오면 그것을 DOM(Document Object Model)로 변경 가능하다.  

* ### **console.dir();**로 해당태그에 모든 정보를 볼 수 있음. 


* ### querySelector();
  인자로는 css selector와 동일하며 첫번째를 가져오겠다는 의미. 


* ### localStorage 
  윈도우에서 정보를 저장해놓기 위한 방법으로 사용된다. 
  * setItem(key,value)
    localStorage에 데이터를 저장하는 법 
  * getItem(key)
    localStorage에서 데이터를 꺼내는 법
    
* ### eventPreventDefault()
  
  아래와 같이 이벤트를 호출했을 경우 **input 값이 사라지는 것** 을 막아줄 수 있음. 
  ```javascript
  function handleSubmit(event){
      event.preventDefault();
      const currentValue = input.value;
      saveName(currentValue);
      printGreeting(currentValue);
  }
  ```
