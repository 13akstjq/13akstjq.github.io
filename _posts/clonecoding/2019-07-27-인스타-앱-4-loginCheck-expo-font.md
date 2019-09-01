---
layout: post
title: "[인스타클론코딩] [App].4- 로그인체크와 expo-font변경"
date: 2019-07-27-14:41:00
author: 한만섭
categories: clonecoding
tags: react instagram app preLoading expo-font
---



* TOC
{:toc}



## expo module변경 



expo에 존재하는 모듈은 모듈의 크기가 커짐에 따라서 각각 외부 모듈로 파생되는 경우가 많습니다. 

이번에 앱 setting 하면서 사용했던 `Font`와 `Asset`도 console창에서 import 방법을 변경하라고 하길래 정리해보려고 합니다.  



* 변경 전 

```js
import {Font,Asset} from 'expo';
```

위와 같이 바로 expo에서 사용했지만 console창을 보니 아래와 같은 메세지가 나왔습니다.  

![1564206328879](../../../../assets/image/1564206328879.png)



* 수정하기 	

위 내용을 보니 `expo-font`와 `expo-asset`을 설치해달라고 하는 것 같습니다. 깔아달라고 하니 설치해주도록 하죠.   

```bash
npm add expo-font expo asset
```

설치를 마쳤다면 import를 해줘야겠죠. 근데 여기서 저는 당연하게 아래와 같이 작성했습니다.  

```js
import {Font} from 'expo-font';
import {Asset} from 'expo-Asset';
```

그랬더니 에러가 발생하더군요.. 위에 내용을 다시보니 `expo-font`는 저런 방식으로 import를 하면 안되더군요.  

```json
import { Asset } from "expo-asset";
import * as Font from "expo-font";
```

![1564206585486](../../../../assets/image/1564206585486.png)

개떡같이 써도 친절하게 알려주는 에러메세지 덕분에 바로 해결할 수 있었습니다.  



***



## Theme 적용하기 

Theme은 앱이나 웹에서 사용하는 색상이나 크기등을 지정해놓는데 사용합니다.  

`Styles/Theme.js`에 지정합니다. 저는 인스타그램이라는 서비스를 클론하기 때문에 아래와같이 초기화했습니다.   

```js
export default {
  bgColor: "#FAFAFA",
  blackColor: "#262626",
  darkGreyColor: "#003569",
  lightGreyColor: "#999",
  redColor: "#ED4956",
  blueColor: "#3897f0"
};

```



만들어 놓은 Theme을 `App.js`에게 제공해야하기 때문에 `styled-components`에 존재하는 `ThemeProvider`를 사용해야합니다.  

```js
import { ThemeProvider } from "styled-components";
```

```react
return loaded && client && isLoggedIn !== null ? (
    <ThemeProvider theme={Theme}>
      <ApolloProvider client={client}>
        {isLoggedIn ? <Text>i'm in</Text> : <Text>I'm out</Text>}
      </ApolloProvider>
    </ThemeProvider>
  ) : (
    <AppLoading />
  );
```

위와 같이 사용할 수 있습니다. 





***



## 로그인 체크하기 

로그인을 체크하는 방법은 웹페이지를 만들었을 때와 동일한 방식인 `isLoggedIn`state를 사용해서 체크하는 방법입니다.

```js
const [isLoggedIn, setIsLoggedIn] = useState(null); 
```

```js
await persistCache({

(...중략...)

//check islogin
      const isLoggedIn = AsyncStorage.getItem("isLoggedIn");
      console.log(isLoggedIn);
      if (isLoggedIn === null || isLoggedIn === false) {
        setIsLoggedIn(false);
      } else {
        setIsLoggedIn(true);
      }
})
```

null이거나 false일 경우에 `logout`상태 , true일 경우에만 `login`상태 

웹페이지에 Localstorage가 있다면 스마트폰에는 `AsyncStorage`가 있습니다. 이것은 비동기 적으로 동작하기 때문에 await를 사용해야합니다.  



### 로그인,로그아웃 함수 만들기 

`App.js`

```js
  logUserOut = () => {
    try {
      AsyncStorage.setItem("isLoggedIn", "false");
      setIsLoggedIn(false);
    } catch (error) {
      console.log(error);
    }
  };

  logUserIn = () => {
    try {
      AsyncStorage.setItem("isLoggedIn", "true");
      setIsLoggedIn(true);
    } catch (error) {
      console.log(error);
    }
  };
```

로그인을 하게되면 state인 `isLoggedIn`도 변경시켜줌과 동시에 `AsyncStorage`에 있는 `isLoggedIn`도 변경을 시켜줘야 합니다. 그래서 위와 같이 작성했습니다. 하지만 `AsyncStorage`를 사용할 때 주의사항이 있습니다.  

#### AsyncStorage 주의사항 

* String 사용하기 

  AsyncStorage에는 setItem을 할 때 boolean값인 true,false를 넣는 것이아니라 "true","false"를 넣어야 합니다.  

  ```js
  AsyncStorage.setItem("isLoggedIn", "true");
  ```

  그렇기 때문에 체크를할 때도 string 으로 check를 해야합니다.  

  ```js
  //check islogin
        const isLoggedIn = await AsyncStorage.getItem("isLoggedIn");
        console.log("isLoggedIn", isLoggedIn);
        if (isLoggedIn === "null" || isLoggedIn === "false") {
          setIsLoggedIn(false);
        } else {
          setIsLoggedIn(true);
        }
  
  ```

  







***



