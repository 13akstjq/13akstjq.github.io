---
layout: post
title:  "[인스타클론코딩] [FrontEnd].11 - follow btn Compoenent 만들기 "
date:   2019-07-06-22:57:00
author: 한만섭
categories: clonecoding
tags: react instagram Header
---








> ### 주의할 점 

아래와 같은 에러가 나오면 {}의심하기 
```error
index.js:1375 Warning: Expected `onClick` listener to be a function, instead got a value of `object` type.
```

`FollowButtonPresenter`  
```javascript
import React from "react";
import Button from "../Button";

export default ({ isFollowing, onClick }) => { // 중괄호 빼먹으면 function이 object가 됨.
  console.log(isFollowing);

  console.log(typeof onClick);
  return (
    <Button
      text={isFollowing === true ? "unFollow" : "Follow"}
      onClick={onClick}
    />
  );
};


```
