---
layout: post
title:  "[ReactNative] 개발과정 이슈 정리 "
date:   2019-06-06 14:41:00
author: 한만섭
categories: reactnative
tags: reactnative 
---

* TOC
{:toc}

> ## 문법 실수 

- `LinearGradient` 를 `LenriearGradient`라고 했는데 직접적인 error log가 뜨지 않아서 찾는데 어려웠음...  
![image](https://user-images.githubusercontent.com/46010705/59009675-8f61b680-8869-11e9-9d14-1fc986c091f7.png)  


- const {isLoaded} = this.state;  
render함수 내에서 const로 state의 변수를 사용할 때 {}사이에 넣어주어야한다. ( react,rn에서의 {}의 의미에 대해서 좀 더 이해할 필요있음)

- `Ionicons` 를 `ionIcons` 낙타형식으로 써서 에러...
 [expo icon 사이트](https://expo.github.io/vector-icons/)
 
- `export default (function or class Name)`
따로 파일형태로 만든 class 혹은 function은 export를 꼭 해주어야한다.  
error log에서 export를 잊은거 아니냐고 알려주니 확인해볼 것.
```
C:\Users\mshan\Desktop\rn\weather-app\node_modules\react-native\Libraries\Core\ExceptionsManager.js:84 Warning: React.createElement: type is invalid -- expected a string (for built-in components) or a class/function (for composite components) but got: object. You likely forgot to export your component from the file it's defined in, or you might have mixed up default and named imports.
```  

- propTypes 대소문자 틀림   
functionName.`P`ropTypes라고 하면 노란 창으로 알려주니까 읽고 수정할 것 
```
Weather.propTypes={
    temp : PropTypes.number.isRequired
}
```
import 방법 
```
import PropTypes from "prop-types";
```  

- render내에서 state사용시 주의
component의 `state`를 render()에서 사용하기 위해서는 아래와 같이 render내에서 this.state를 변수로 갖고 있어야한다. 
```
const {newToDo} = this.state;
```

