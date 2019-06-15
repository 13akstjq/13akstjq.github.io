---
layout: post
title:  "[JS] vanillaJS 02 - JS event  "
date:   2019-06-15 13:37:00
author: 한만섭
categories: js
tags: js function event callback
---

event에 달려있는 callback 함수가 언제 호출되는지 알아보려고 한다. 

> ### 이벤트가 발생할 때 함수 호출 
  
  ```javascript
  function handleResize(){
    console.log("resize!!!!");
  }
  window.addEventListener("resize",handleResize);
  ```
  
  위와 같이 코드를 작성하면 화면이 새로고침되어도 console이 찍히지 않고 화면 크기가 조정될 때만 console에 **resize!!!!** 가 찍힌다. 
  　  
  
  ***
  
  　  
  
> ### 이벤트가 발생하기도 전에 함수 호출 
  
  ```javascript
  function handleResize(){
    console.log("resize!!!!");
  }
  window.addEventListener("resize",handleResize());
  ```
  이렇게 작성하면 화면이 새로고침 되자마자 console에 **resize!!!!** 가 찍히는 것을 볼 수 있다.  
  그리고 이렇게 작성하면 callback 형식이 아니라서 화면크기가 바뀌어도 console에 찍히지 않는다. 
　  
  
  ***
  
  　  
　  
> ### tag에 event추가하기 
  원하는 태그에 이벤트를 추가해야할 일은 필수이다. 그럴땐 **addEventListener**를 사용하자.  
  
  아래 코드는 **id**가 **title**인 tag에 **click** event를 추가하는 코드입니다. 역시 콜백에는 `()`를 붙히지 않습니다. 
  ```javascript
  const title = document.querySelector("#title");
  function handleClick(){
    title.style.color = "red";
  }
  title.addEventListener("click",handleClick);
  ```
  　  
  
  ***
  
  　  
> ### color를 colsole에 찍으면 **rgb(41, 128, 185)** 형태로 나오니까 값이 적용이 안되는 것들은 console찍어보기!  

  [event MDN](https://developer.mozilla.org/ko/docs/Web/Events)
  　  
  
  ***
  
  　  
> ###  event.preventDefault();
  * 이벤트가 호출되었을 때 **input**값이 사라지는 것을 막아줄 수 있음. 이것을 하지않으면 event호출이 document까지 올라가기 때문에 
  
  ```javascript
  function handleSubmit(event){
      event.preventDefault();
      const currentValue = input.value;
      saveName(currentValue);
      printGreeting(currentValue);
  }
  ```
　  
  
  ***
  
  　  
