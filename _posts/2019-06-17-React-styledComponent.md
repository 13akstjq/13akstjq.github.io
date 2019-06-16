---
layout: post
title:  "[React] Styled Component를 사용하는 이유"
date:   2019-06-17 05:39:00
author: 한만섭
categories: react
tags: react styledComponent
---

> ## 시작하기전에...
  이 글에서는 React와 React-Native에서 사용하던 `ClassName`과 같이 클래스 방식의 코딩이 아닌 StyledComponent 이름 그대로 
  스타일이 내장된 컴포넌트를 사용하는 이유를 적어보려고 합니다. styledComponent의 사용방법에 대해서는 따로 글을 쓰도록 하겠습니다. 
  
  
> ### StyledComponent의 장점 
  styledComponent란 스타일이 내장되어 있는 컴포넌트로서 npm을 이용해 추가할 수 있습니다. 
  ```
  npm add styled-components
  ```
  
  1. 클래스를 만들 필요가 없다. 
    * 대부분 클래스 형태로 css를 지정했는데 이것을 사용하면 class를 안쓰고 css를 적용할 수 있기 때문에 편리한 것 같습니다. 
    
  2. CSS파일이 필요없다. 
    * CSS파일을 만들지 않고 **.js**파일에 직접 코딩해서 CSS파일이 많아지는 것을 막을 수 있다고 하는데, 이 부분은 사실 잘 모르겠습니다. 
    객체지향, 함수적 프로그래밍이 보기 더 쉽고 가독성이 좋은 것이 아닌가 하는 의문이 드는데 한 파일에 CSS와 js를 도무 코딩한다면 별로 보기 
    좋지 않을 것 같습니다. 하지만 제가 잘못알고 있는게 분명하기 때문에 넘어가도록 하겠습니다. 
    
  3. RN과 CSS를 공유할 수 있다.  
    * RN에서는 style을 작성할 때 CSS형식과 다르게 작성했습니다. 하지만 StyledComponent를 사용하면 CSS형식으로 작성하기 때문에 호환성이 좋습니다. 
  
