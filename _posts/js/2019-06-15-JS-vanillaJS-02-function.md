---
layout: post
title:  "[JS] vanillaJS 02 - JS function  "
date:   2019-06-15 12:13:00
author: 한만섭
categories: js
tags: js function DOM
---

* TOC
{:toc}


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
    

**JSON** 라이브러리에는 **Object** 와 **Json** 간의 변환을 도와주는 function이 존재합니다.  


> ### 1. JSON.stringify
   이 함수는 Object를 String 형태로 변환해주는 메소드 입니다. LocalStorage.setItem()을 통해서 데이터를 넣을 때 **Object**형태로 넣을 수 없기 
   때문에 **String**형태로 변환해야 합니다. 그 과정에서 이 메소드를 사용하면 `Object -> String`을 손쉽게 할 수 있습니다.


> ### 2. JSON.parse
  이 함수는 **String**형식의 파일을 **Object** 형태로 변환해줍니다. LocalStorage에 저장해야하는 데이터들은 객체(Object)가 아니라 String 형태여야
  하기 때문에 LocalStorage에서 데이터를 꺼내면 String 형태입니다. 그것을 Object로 사용해야할 때가 많은데 그 때 이 메소드를 사용하면 
  `Stirng -> Object`를 할 수 있습니다. 
  
  
  > ### a tag로 png파일 다운로드 하기 
a태그는 다운로드를 할때도 사용할 수 있습니다. 제가 다운로드 받을 파일은 캔버스로 그린 그림입니다. canvas의 그림에 대한 URL은 `toDataURL()`로 
구할 수 있습니다. 그리고 `a`tag를 만들고 그 a tag에 **link** 와 **download**를 추가해줘야 합니다. **link** 는 위에서 toDataURL로 구한 URL주소입니다. 
**download**는 다운로드 받아질 때의 파일명입니다. 마지막으로 a 태그 객체인 link를 강제로 click해주는 트릭을 이용하면 해당함수로 파일을 다운로드 할 수 
있습니다. 

```javascript
function saveFile(event){
    const image = canvas.toDataURL();
    console.log(image);
    const link = document.createElement('a');
    link.href = image;
    link.download = 'picture';
    console.log(link);
    link.click();
}
```
