---
layout: post
title: "Manstagram - app.2 - cache 사용해서 preloading하기"
date: 2019-10-14-15:23:00
author: 한만섭
categories: manstagram
tags: React ReactHooks ReactNative Graphql Prisma Apollo
---



* TOC
{:toc}


### 1. preLoading이란 ?

`preLoading`은 사용자가 앱을 켰을 때 로고 및 이미지가 나와있지 않는 것보다 다 `loading`이 되어있는 상태에서 보는 것이 더 완벽한 앱이라는 인식을 받기 때문에 미리 `data`를 `load`해서 보여주는 것입니다.

#### 1.1 Icon preLoading 하기

[expo 공식 사이트](https://docs.expo.io/versions/latest/guides/using-custom-fonts/)에 font를 `loadAsync` 하는 방법이 나와 있으니 보고 따라하도록 하겠습니다.

![1564070827901](../../../../assets/image/1564070827901.png)

`App.js`

```js
import React, { useState, useEffect } from "react";
import { StyleSheet, Text, View } from "react-native";
import { AppLoading, Font } from "expo"; // expo의 Font import 하기
import { Ionicons } from "@expo/vector-icons";
export default function App() {
  const [loaded, setLoaded] = useState(false);

  const preLoad = async () => {
    try {
      await Font.loadAsync({
        // font를 load하는 곳
        ...Ionicons.font
      });
      setLoaded(true); // load를 다 하면 state변경
    } catch (error) {
      console.log(error);
    }
  };

  useEffect(() => {
    preLoad();
  }, []);

  return loaded ? (
    <Text>Open up App.console.log(); your app!</Text>
  ) : (
    <AppLoading />
  );
}
```

`Ionicons`를 load하고 나서 화면을 보여주기 위해 `useEffect(ComponentDidMount)`에서 load를 해오는 함수를 호출합니다.

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="4307878116"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

#### 1.2 logo preLoading 하기

[expo 공식사이트](https://docs.expo.io/versions/v33.0.0/guides/assets/)에 local에 있는 이미지에 대해서 미리 load하는 것에 대해 알려주고 있습니다.

`require`를 사용하면 불러올 수 있다고 합니다.

![1564072405294](../../../../assets/image/1564072405294.png)

`App.js`

```js
await Asset.loadAsync(require("./assets/logo.png"));
```

위에 작성한 코드를 `App.js`에 넣어주면 Icon과 Logo에 대한 preLoad를 진행 할 수 있습니다.

------

#### 1.3 느낀 점 

`expo`의 `Font,Assets`를 import 해서 `loadAsync`와 `require`를 이용해 데이터를 미리 load하는 방식을 사용했는데, [API DOC]를 자주 읽는 습관을 들여야 할 것 같습니다. 평소에 잘 읽지 않으니 DOC를 봐도 이해가 안되는 게 너무 많은 것 습니다..



### 2 .cache 사용하기 

```js
import { InMemoryCache } from "apollo-cache-inmemory"; //새로운 cache를 만들기 위함.
import { persistCache } from "apollo-cache-persist"; // 캐시 유지 
import apolloClientOptions from "./apollo";
import ApolloClient from "apollo-boost"; // ApolloClient를 쉽게 setting하기 위함
import { ApolloProvider } from "react-apollo-hooks"; //react-apollo에서 react-hooks를 사용하기 위함. 

```

client에서 작업하기 위해 apolloClient를 사용해야하는데 쉬운 설정을 하기 위해 `apollo-boost`를 사용했습니다. 

- apollo-boost :  client에서 작업하기 위해 apolloClient를 사용해야하는데 쉬운 설정을 하기 위해 `apollo-boost`를 사용했습니다.  
- ApolloProvider : apollo-boost의 apolloClient로 만든 `client`를 제공해줄때 사용하는 것. 이번에는 `react-hooks`와 같이 사용할 것이기 때문에 `react-apollo-hooks`의 `apolloProvider`사용   
- persistCache : apollo에서 제공해주는 `cache`를 저장해주고 유지해주는 모듈. 
- apollo-cache-inmemory : 메모리상에 cache를 만들기 위해 사용하는 모듈. 



------

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="4307878116"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

- cache를 만들어야함(`inMemoryCache`) 

  ```js
  const cache = new InMemoryCache();
  ```

  

- 만든 cache를 유지시켜야함(`persistCache`) 

  ```js
  import { Text, View, AsyncStorage } from "react-native";
  ```

  

  ```js
   await persistCache({
          cache,
          storage: AsyncStorage
        });
  ```

  

- cache를 가지고 있는 client 만들기 (`ApolloClient`)

  ```js
   const client = new ApolloClient({
          cache,
          ...apolloClientOptions
        });
  ```

​	

- Client 넘겨주기 (`ApolloProvider`)

  ```js
   <ApolloProvider client={client}>
        <Text>Open up App..log(); your app!</Text>
      </ApolloProvider>
  ```



------

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="4307878116"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

`App.js`

```js
import React, { useState, useEffect } from "react";
import { Text, View, AsyncStorage } from "react-native";
import { AppLoading, Font, Asset } from "expo";
import { Ionicons } from "@expo/vector-icons";
import { InMemoryCache } from "apollo-cache-inmemory"; //새로운 cache를 만들기 위함.
import { persistCache } from "apollo-cache-persist"; // 캐시 유지
import apolloClientOptions from "./apollo";
import ApolloClient from "apollo-boost"; // ApolloClient를 쉽게 setting하기 위함
import { ApolloProvider } from "react-apollo-hooks"; //react-apollo에서 react-hooks를 사용하기 위함.

export default function App() {
  const [loaded, setLoaded] = useState(false);
  const [client, setClient] = useState(null);
  const preLoad = async () => {
    try {
      await Font.loadAsync({
        ...Ionicons.font
      });
      await Asset.loadAsync(require("./assets/logo.png"));
      const cache = new InMemoryCache();
      await persistCache({
        cache,
        storage: AsyncStorage
      });
      const client = new ApolloClient({
        cache,
        ...apolloClientOptions
      });
      setClient(client);
      setLoaded(true);
    } catch (error) {
      console.log(error);
    }
  };

  useEffect(() => {
    preLoad();
  }, []);

  return loaded ? (
    <ApolloProvider client={client}>
      <Text>Open up App..log(); your app!</Text>
    </ApolloProvider>
  ) : (
    <AppLoading />
  );
}

```



------



## setting 요약



컴포넌트가 마운드 될때 (`useEffect`)사용해서 `preLoading`을 했습니다.  preLoad는 비동기 함수입니다. preload가 하는 일은 크게 세가지로 나눌 수 있습니다. 첫번째로, `font`를 Load하는 동작입니다. 두번째로 `assets`를 Load합니다.  마지막으로는 `cache`를 만들고 그 `cache`를 가지고 있는 `client`를 만드는 것입니다.  



- `font` load하기 

  제가 사용하는 font는 `@expo/vector-icons`이기 때문에 아래 내용을 import 합니다.  

  ```js
  import { Ionicons } from "@expo/vector-icons";
  ```

  ```js
   await Font.loadAsync({
          ...Ionicons.font
        });
  ```

- `Assets` load하기 

  인스타그램의 logo를 load하기위해 사용합니다.  

  ```js
   await Asset.loadAsync(require("./assets/logo.png"));
  ```

  여러개의 loacal image를 load하고 싶을 때는 `[]`를 사용해서 load할 수도  있습니다.  

  <script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
  <ins class="adsbygoogle"
       style="display:block; text-align:center;"
       data-ad-layout="in-article"
       data-ad-format="fluid"
       data-ad-client="ca-pub-4877378276818686"
       data-ad-slot="4307878116"></ins>
  <script>
       (adsbygoogle = window.adsbygoogle || []).push({});
  </script>

- `cache`만들기 

  사용자가 앱을 킬 때마다 load를 하는 모습을 보여주지 않기 위해 `cache`에 데이터를 저장해줄 것입니다.   

  ```js
  import { InMemoryCache } from "apollo-cache-inmemory"; //새로운 cache를 만들기 위함.
  import { persistCache } from "apollo-cache-persist"; // 캐시 유지
  ```

  ```js
   const cache = new InMemoryCache();
        await persistCache({
          cache,
          storage: AsyncStorage
        });
        
  ```

- cache를 가지고 있는 client만들기 

  ```js
  const client = new ApolloClient({
          cache,
          ...apolloClientOptions
        });
  ```

  ```js
  <ApolloProvider client={client}>
        <Text>Open up App..log(); your app!</Text>
      </ApolloProvider>
  ```

  