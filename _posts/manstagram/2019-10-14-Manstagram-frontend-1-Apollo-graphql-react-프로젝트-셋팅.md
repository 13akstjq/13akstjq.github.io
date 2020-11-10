---
layout: post
title: "Manstagram - frontend.1 - Apollo graphql react 프로젝트 셋팅"
date: 2019-10-14-15:23:00
author: 한만섭
categories: manstagram
tags: React graphql apollo
---



* TOC
{:toc}


![image](https://user-images.githubusercontent.com/46010705/66883534-3d941e80-f009-11e9-8f09-c9e4bd61c091.png)

라이브러리를 모두 설치한 프로젝트에서 해야할 내용인 **GlobalCSS**와 **Theme**로 초기 스타일 셋팅을 어떻게 했는지 정리한 글입니ㄷ.　  

***



### 1. 프로젝트 셋팅 

- npx로 create-react-app 프로젝트 생성 

  ```
  npx create-react-app prismagram-frontend
  ```

  ![image](https://user-images.githubusercontent.com/46010705/60510911-9d411500-9d0b-11e9-9c8f-dca0e88d126e.png)

- react 프로젝트 정리 

  아래와 같은 프로젝트 형태가 되도록 필요없는 파일을 삭제합니다.  
  ![image](https://user-images.githubusercontent.com/46010705/60510983-d24d6780-9d0b-11e9-9202-e04c8c823e37.png)

- git 연동하기 

  react프로젝트와 github연동은 보러가기

***



### 2. 라이브러리 설치

- 컴포넌트 스타일링 : **styled-components** 
- 사이트 이름 꾸미기 : **react-helmet**
-  : **styled-reset react-toastify**
- 경로 지정 : **react-router-dom**
- react + graphql + apollo클라이언트 만들기: **apollo-boost react-apollo-hooks**
- back연결? : **graphql**  

```bash
  npm add styled-components react-router-dom graphql react-apollo-hooks apollo-boost react-helmet
  npm add styled-reset react-toastify
```

***

### 3. GlobalStyles.js

styled-components에서 제공하는 모든 tag에 스타일을 적용할 수 있는 라이브러리인 **createGlobalStyle**을 이용합니다.  

- src/Styles/GlobalStyles.js

  **styled-reset** 에 있는 **reset**을 사용합니다. 기존 styled-components 방식으로 작성합니다.  

  ```js
  import { createGlobalStyle } from "styled-components";
  import reset from "styled-reset";
  
  export default createGlobalStyle`
      ${reset};
      * {
          box-sizing : border-box;
      }
  `;
  
  ```

------

### 4. Theme 

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<!-- n잡 블로그 사각형 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="2552901794"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

Theme.js에서는 자주 사용되는 색상이나 패턴을 저장할 것입니다. 그래서 단순히 작성하고 **export default**로 내보내주면 됩니다.  

- src/Styles/Theme.js

  ```js
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

------

### 5. App.js 

App.js에서는 위에서 작성한 **GlobalStyles**와 **Theme**를 불러서 사용하는 곳입니다. Theme은 **ThemeProvider**를 통해서 컴포넌트에게 전달됩니다. 
GlobalStyles는 import 후 tag형태로 작성해놓으면 적용됩니다. App.js는 함수형으로 제작하였습니다.  

- App.js

  ```js
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

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<!-- n잡 블로그 사각형 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="2552901794"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

### 6. React-Router-Dom

프로젝트의 페이지 경로들을 결정해주는 Route를 작성할 수 있습니다. 간략하게 설명하자면 **Router.js**에서 Routes(Feed, Auth, Profile, EditProfile, Explorer)들 중에서 어디를 보여줄지 정해주는 것입니다. Routes들에는 보여져야할 페이지가 들어가는 것입니다. 그래서 아래와 같이 파일을 
만들면 됩니다.  
AppRouter가 경로를 결정해주는 파일이고, Routes에 있는 파일들이 실제 인스타그램에서 보여지는 페이지들입니다.  
전체적인 흐름을 돕기위해 얘기하자면, 만들 프로젝트에 보여질 페이지들을 **Routes**로 정해놓고 **Routes**에는 많은 **Components**들이 보여지고 가려지는 것입니다.  
![image](https://user-images.githubusercontent.com/46010705/60531046-15700080-9d35-11e9-9218-b1eb8ef6fbc1.png)

각 Routes에는 확인을 위한 코드를 작성해보도록 하겠습니다.  

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<!-- n잡 블로그 사각형 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="2552901794"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

```js
  export default () => "EditProfile";
```

- Router.js  

Router.js를 만들 때 필요한 것은 아래와 같이 있습니다.  

```js
  import { HashRouter as Router, Route, Switch } from "react-router-dom";
  import React from "react";
  import Feed from "../Routes/Feed";
  import Auth from "../Routes/Auth";
  import PropType from "prop-types";
```

간단하게 흐름을 말씀드리자면,  
Router를 만들고 Router의 Props에 따라 Router안에 어떤 **Route**를 넣는지 결정됩니다. Route는 **path**와 **component**를 정해줘서 경로를 바꿔주는 역할을 합니다. Route를 Router가 감싸고 Proptype을 지정해주고 **export default** 해주면 App.js에서 사용할 수 있습니다.  

```js
  import { HashRouter as Router, Route, Switch } from "react-router-dom";
  import React from "react";
  import Feed from "../Routes/Feed";
  import Auth from "../Routes/Auth";
  import PropType from "prop-types";
  const LogedInRouter = () => (
    <>
      <Route exact path="/" component={Feed} />
    </>
  );
  const LogedOutRouter = () => (
    <>
      <Route exact path="/" component={Auth} />
    </>
  );

  const AppRouter = ({ isLoggedIn }) => (
    <Router>
      <Switch>{isLoggedIn ? <LogedInRouter /> : <LogedOutRouter />}</Switch>
    </Router>
  );

  AppRouter.prototype = {
    isLoggedIn: PropType.bool.isRequired
  };

  export default AppRouter;
```

- App.js

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<!-- n잡 블로그 사각형 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="2552901794"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

App.js에서는 기존 코드에서 AppRouter를 import 해서 사용하는 것밖에 추가되지 않습니다.  

```js
  import React from "react";
  import GlobalStyles from "../Styles/GlobalStyles";
  import { ThemeProvider } from "styled-components";
  import Theme from "../Styles/Theme";
  import AppRouter from "./AppRouter";

  export default () => (
    <ThemeProvider theme={Theme}>
      <>
        <GlobalStyles />
        <AppRouter isLoggedIn={true} />
      </>
    </ThemeProvider>
  );  
```

  여기서 주의할 점은 `<></>`로 **ThemeProvider**안에를 감싸주는 것입니다.  

　  

***



