---
layout: post
title:  "[JS] JSON라이브러리를 이용해 Object <-> String 변환하기  "
date:   2019-06-16 14:39:00
author: 한만섭
categories: js
tags: js JSON 
---

**JSON** 라이브러리에는 **Object** 와 **Json** 간의 변환을 도와주는 function이 존재합니다.  


> ### 1. JSON.stringify
   이 함수는 Object를 String 형태로 변환해주는 메소드 입니다. LocalStorage.setItem()을 통해서 데이터를 넣을 때 **Object**형태로 넣을 수 없기 
   때문에 **String**형태로 변환해야 합니다. 그 과정에서 이 메소드를 사용하면 `Object -> String`을 손쉽게 할 수 있습니다.


> ### 2. JSON.parse
  이 함수는 **String**형식의 파일을 **Object** 형태로 변환해줍니다. LocalStorage에 저장해야하는 데이터들은 객체(Object)가 아니라 String 형태여야
  하기 때문에 LocalStorage에서 데이터를 꺼내면 String 형태입니다. 그것을 Object로 사용해야할 때가 많은데 그 때 이 메소드를 사용하면 
  `Stirng -> Object`를 할 수 있습니다. 
