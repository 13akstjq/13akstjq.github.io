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

![1564070827901](../../../../../바탕화면 정리/assets/image/1564070827901.png)

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

#### 1.2 logo preLoading 하기

[expo 공식사이트](https://docs.expo.io/versions/v33.0.0/guides/assets/)에 local에 있는 이미지에 대해서 미리 load하는 것에 대해 알려주고 있습니다.

`require`를 사용하면 불러올 수 있다고 합니다.

![1564072405294](../../../../../바탕화면 정리/assets/image/1564072405294.png)

`App.js`

```js
await Asset.loadAsync(require("./assets/logo.png"));
```

위에 작성한 코드를 `App.js`에 넣어주면 Icon과 Logo에 대한 preLoad를 진행 할 수 있습니다.

------

#### 1.3 느낀 점 

`expo`의 `Font,Assets`를 import 해서 `loadAsync`와 `require`를 이용해 데이터를 미리 load하는 방식을 사용했는데, [API DOC]를 자주 읽는 습관을 들여야 할 것 같습니다. 평소에 잘 읽지 않으니 DOC를 봐도 이해가 안되는 게 너무 많은 것 습니다..