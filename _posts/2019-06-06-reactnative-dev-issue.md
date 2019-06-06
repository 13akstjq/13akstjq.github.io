---
layout: post
title:  "[ReactNative] 개발과정 이슈 정리 "
date:   2019-06-06 14:41:00
author: 한만섭
categories: reactnative
tags: reactnative 
---

> ## 문법 실수 

- `LinearGradient` 를 `LenriearGradient`라고 했는데 직접적인 error log가 뜨지 않아서 찾는데 어려웠음...  
![image](https://user-images.githubusercontent.com/46010705/59009675-8f61b680-8869-11e9-9d14-1fc986c091f7.png)  


- const {isLoaded} = this.state;  
render함수 내에서 const로 state의 변수를 사용할 때 {}사이에 넣어주어야한다. ( react,rn에서의 {}의 의미에 대해서 좀 더 이해할 필요있음)

- `Ionicons` 를 `ionIcons` 낙타형식으로 써서 에러...
 [expo icon 사이트](https://expo.github.io/vector-icons/)
 
