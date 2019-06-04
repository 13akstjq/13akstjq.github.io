---
layout: post
title:  "[CSS] Flex Box  "
date:   2019-06-04 15:30:00
author: 한만섭
categories: html/css
tags: css Flex
---

> ## Flex box

> ### flex-direction
Flex를 사용하기 위해서는 상위태그와 하위태그가 존재해야한다.   
상위 태그의 style의 display : flex로 해주어야 한다.  
그렇다면 정렬 방식은 어떻게 조절할까?  정렬하는 방식을 정의하는 것이 `flex-direction`이다.

- 상위 태그에 수평방향(->) 으로 넣고 싶은 경우 
```
flex-direction : row; 
```
위 코드를 넣지 않더라도 default값이 row이다.  

- 상위 태그의 수직방향 (v) 으로 넣고 싶은 경우
```
flex-direction : column;
```

- 반대로 넣고 싶은 경우 reverse를 이용하면 된다. 
```
flex-direction : row-reverse;
```


```javascript
<!doctype>
<html>
<head>
    <style>
        .container{
            background-color: powderblue;
            height:200px;
            display:flex;
            flex-direction:row;
        }
        .item{
            background-color: tomato;
            color:white;
            border:1px solid white;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="item">1</div>
        <div class="item">2</div>
        <div class="item">3</div>
        <div class="item">4</div>
        <div class="item">5</div>
    </div>
</body>
</html>
```



> ## 2. grow & shrink
화면의 크기를 조절하다보면 원하는 부분의 크기만 조절가능하게 하고싶을 때가 있다.  
그럴 때 grow 와 shrink를 사용하면 된다. 

- tag에서 특정 자식의 스타일을 변경하고 싶을 경우 
```
.item:nth-child(1){
  flex-basis: 150px;
  flex-shrink: 1;
}
```
클래스가 item 인 tag중에 두번째의 크기(basis)와 shrink 를 1로 지정하겠다.   


- tag의 기본 크기를 지정해주는 방법 
```
flex-basis : 150px;
```  
크기의 기준이 width 인지 height 인지는 상위 태그의 `flex-direction:row;이면 width `flex-direction : column;`이면 height 이다. 


- grow
grow를 하게되면 상위태그의 크기에 맞춰진다. ( grid의 비율을 의미하는 것 같음 )

- shrink 
화면의 크기를 조정할 경우 tag의 크기가 작아지는 비율을 의미한다. 

- order
html에 작성한 코드의 순서가 아닌 style에서 적용한 order의 순서에 따라 그려지는 순서를 결정해준다. 



