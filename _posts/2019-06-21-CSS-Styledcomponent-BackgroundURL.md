---
layout: post
title:  "[CSS] StyledComponent Backgound-image url에 props넣기 "
date:   2019-06-21-00:45:00
author: 한만섭
categories: css
tags: styledcomponent
---

### ...
  바로 코드를 보여드리겠습니다. 
  * component props 값 넣어주기 
    ```
    <Card img={img}>
    ```
    
  * props 값을 url로 사용하기 
    ```
     background-image : ${props => `url(${props.img})`};
    ```
