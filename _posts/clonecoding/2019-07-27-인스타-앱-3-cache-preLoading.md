---
jslayout: post
title: "[인스타클론코딩] [App].3 - cache preLoading "
date: 2019-07-26-20:11:00
author: 한만섭
categories: clonecoding
tags: react instagram app preLoading cache프로
---



### cache를 위한 import 

```js

import { InMemoryCache } from "apollo-cache-inmemory"; //새로운 cache를 만들기 위함.
import { persistCache } from "apollo-cache-persist"; // 캐시 유지 
import apolloClientOptions from "./apollo";
import ApolloClient from "apollo-boost"; // ApolloClient를 쉽게 setting하기 위함
import { ApolloProvider } from "react-apollo-hooks"; //react-apollo에서 react-hooks를 사용하기 위함. 

```

client에서 작업하기 위해 apolloClient를 사용해야하는데 쉬운 설정을 하기 위해 `apollo-boost`를 사용했습니다. 

* apollo-boost :  client에서 작업하기 위해 apolloClient를 사용해야하는데 쉬운 설정을 하기 위해 `apollo-boost`를 사용했습니다.  
*  ApolloProvider : apollo-boost의 apolloClient로 만든 `client`를 제공해줄때 사용하는 것. 이번에는 `react-hooks`와 같이 사용할 것이기 때문에 `react-apollo-hooks`의 `apolloProvider`사용   
* persistCache : apollo에서 제공해주는 `cache`를 저장해주고 유지해주는 모듈. 
* apollo-cache-inmemory : 메모리상에 cache를 만들기 위해 사용하는 모듈. 



***



* cache를 만들어야함(`inMemoryCache`) 

  ```js
  const cache = new InMemoryCache();
  ```

  

* 만든 cache를 유지시켜야함(`persistCache`) 

  ```js
  import { Text, View, AsyncStorage } from "react-native";
  ```

  

  ```js
   await persistCache({
          cache,
          storage: AsyncStorage
        });
  ```

  

* cache를 가지고 있는 client 만들기 (`ApolloClient`)

  ```js
   const client = new ApolloClient({
          cache,
          ...apolloClientOptions
        });
  ```

​	

* Client 넘겨주기 (`ApolloProvider`)

  ```js
   <ApolloProvider client={client}>
        <Text>Open up App..log(); your app!</Text>
      </ApolloProvider>
  ```



***



#### `App.js`전체 코드 

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



***



## setting 요약



컴포넌트가 마운드 될때 (`useEffect`)사용해서 `preLoading`을 했습니다.  preLoad는 비동기 함수입니다. preload가 하는 일은 크게 세가지로 나눌 수 있습니다. 첫번째로, `font`를 Load하는 동작입니다. 두번째로 `assets`를 Load합니다.  마지막으로는 `cache`를 만들고 그 `cache`를 가지고 있는 `client`를 만드는 것입니다.  



* `font` load하기 

  제가 사용하는 font는 `@expo/vector-icons`이기 때문에 아래 내용을 import 합니다.  

  ```js
  import { Ionicons } from "@expo/vector-icons";
  ```

  ```js
   await Font.loadAsync({
          ...Ionicons.font
        });
  ```

* `Assets` load하기 

  인스타그램의 logo를 load하기위해 사용합니다.  

  ```js
   await Asset.loadAsync(require("./assets/logo.png"));
  ```

  여러개의 loacal image를 load하고 싶을 때는 `[]`를 사용해서 load할 수도  있습니다.  

* `cache`만들기 

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

* cache를 가지고 있는 client만들기 

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

  

