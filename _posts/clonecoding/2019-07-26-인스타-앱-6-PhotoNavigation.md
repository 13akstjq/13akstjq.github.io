---
layout: post
title: "[인스타클론코딩] [App].6- Navigation 정리"
date: 2019-07-28-15:17:00
author: 한만섭
categories: clonecoding
tags: react instagram app Navigation
---



> ## Navigation 정리

***



> ### 1. AuthNavagation 

* AuthHome
* Login
* Signup
* Confirm

위 네가지 컴포넌트를 가지고 있는 `Navigation`입니다.  



> ### 2. TabNavigation  

* Home
* Search
* Notifications
* Profile

위 네가지 컴포넌트를 가지고 있는 `Navigation`입니다.  



> ### 3. PhotoTabs

* TakePhoto
* selectPhoto

이것들은 `topTabNavigation`으로 사용됩니다.  



> ### 4. UpLoad



3.photo Tab과 4. upload를 하나의 stackNavigation으로 만들어 사용합니다.  







> ### Navigation Scope 

```js
  navigationOptions: {
      tabBarOnPress: ({ navigation }) => {
        navigation.navigate("PhotoNavigation");
      }
    }
```

`.navigate()`로 이동 할 수 있는 곳은 해당 navigation안에 있는 곳만 이동 가능 하다.  

photoNavation 에서 AuthNavigation으로 이동하는 것은 안됨.