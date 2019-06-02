---
layout: post
title:  "[React] react에서 prop-type 사용하기 "
date:   2019-06-02
author: 한만섭
categories: react
tags: react proptype
---

### proptype이란?

proptype이란 component에 들어온 값이 유효한 값인지 prop의 type을 미리 정의해놓는 것이다.  
미리 정해놓을 경우 잘못된 값이 들어오는 것을 막아줄 수 있다. Debug하는데 유용하게 쓰일 것으로 생각한다.  

### react에서 proptype사용하기 

원래 react에는 react.proptype로 proptype을 사용할 수 있었다고 한다. 하지만 버전이 바뀌면서 이제는 react와 별개의 라이브러리로 제공한다.  
그렇기 때문에 따로 설치를 해주어야 한다.  

### proptype 설치하기 

```
npm install -save prop-types
```

### proptype 적용하기

proptype을 사용하는 모든 파일에 import를 해야한다. 라이브러리를 불러와야하기 때문에
```
import PropTypes from 'prop-types';
```


### proptype 사용하기 

title과 poster는 string 형태의 prop임과 title은 필수임을 명시 
```
 static propTypes = {
        title : PropTypes.string.isRequired, 
        poster : PropTypes.string
    }
```

### 잘못된 proptype을 사용하면 ?

아래와 같이 영화 제목을 number로 지정할 경우 에러가 발생한다.
```
static propTypes = {
        title : PropTypes.number, 
        poster : PropTypes.string
    }
```  
```console
index.js:1375 Warning: Failed prop type: Invalid prop `title` of type `string` supplied to `Movie`, expected `number`.
    in Movie (at App.js:27)
    in App (at src/index.js:7)
```
