---
layout: post
title:  "[인스타클론코딩] [FrontEnd].2 - GlobalStyles & Theme 초기화  "
date:   2019-07-02-22:12:00
author: 한만섭
categories: clonecoding
tags: react instagram GlobalCSS 
---


> ## 정리할 내용 

이번 포스팅에서는 라이브러리를 모두 설치한 프로젝트에서 해야할 내용인 **GlobalCSS**와 **Theme**를 정리해보려고 합니다. **GlobalCSS**는 모든 태그에 적용
되는 style을 작성하는 것이고, **Theme**은 프로젝트에 사용될 **색상코드,스타일** 과 같은 자주 사용되는 정보들을 저장해놓는 곳입니다.  
　  

***

　  
> ### GlobalStyles

styled-components에서 제공하는 모든 tag에 스타일을 적용할 수 있는 라이브러리인 **createGlobalStyle**을 이용합니다.  
  
* src/Styles/GlobalStyles.js
  
  **styled-reset** 에 있는 **reset**을 사용합니다. 기존 styled-components 방식으로 작성합니다.  
  ```
  import { createGlobalStyle } from "styled-components";
  import reset from "styled-reset";

  export default createGlobalStyle`
      ${reset};
      * {
          box-sizing : border-box;
      }
  `;

  ```
　  

***

　  
> ### Theme.js

Theme.js에서는 자주 사용되는 색상이나 패턴을 저장할 것입니다. 그래서 단순히 작성하고 **export default**로 내보내주면 됩니다.  

* src/Styles/Theme.js

  ```
  const BOX_BORDER = "border: 1px solid #e6e6e6;";
  const BOX_RADIUS = "4px";

  export default {
    bgColor: "#FAFAFA",
    blackColor: "#262626",
    darkGreyColor: "#003569",
    lightGreyColor: "#999",
    redColor: "#ED4956",
    blueColor: "#3897f0",
    boxBorder: "border: 1px solid #e6e6e6;",
    boxRadius: "4px",
    whiteBox: `
          ${BOX_BORDER},
          ${BOX_RADIUS},
          background-color : white
      `
  };
  ```
　    

***

　  
> ### App.js

App.js에서는 위에서 작성한 **GlobalStyles**와 **Theme**를 불러서 사용하는 곳입니다. Theme은 **ThemeProvider**를 통해서 컴포넌트에게 전달됩니다. 
GlobalStyles는 import 후 tag형태로 작성해놓으면 적용됩니다. App.js는 함수형으로 제작하였습니다.  

* App.js

  ```
  import React from "react";
  import GlobalStyles from "../Styles/GlobalStyles";
  import { ThemeProvider } from "styled-components";
  import Theme from "../Styles/Theme";

  export default () => (
    <ThemeProvider theme={Theme}>
      <GlobalStyles />
    </ThemeProvider>
  );

  ```
　    
  
***

　  
> ## 배운 점 
  
* 아래 사진처럼 js파일의 경우 파일이름으로 자동완성이 가능하다.  
  ![image](https://user-images.githubusercontent.com/46010705/60516161-25c5b280-9d18-11e9-981a-feced7296e97.png)

  
