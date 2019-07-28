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



> ## 2. MainNavigation 

### 2-1. TabNavigation  

* Home
* Search
* Notifications
* Profile

위 네가지 컴포넌트를 가지고 있는 `Navigation`입니다.  



### 2-2. PhotoNavigation

* **photoTabs**

  * TakePhoto
  * selectPhoto

  이것들은 `topTabNavigation`으로 사용됩니다.  

* **UpLoad**
  * 이미지를 업로드하는 컴포넌트입니다. 

photo Tab과 upload를 하나의 stackNavigation으로 만들어 사용합니다.  



### 2-3. MessagesNavigation

* Messages
* Message



***



### 흐름도 

tabnavigation의 add를 누르면 





> ### Navigation Scope 

```js
  navigationOptions: {
      tabBarOnPress: ({ navigation }) => {
        navigation.navigate("PhotoNavigation");
      }
    }
```

`.navigate()`로 이동 할 수 있는 곳은 해당 navigation안에 정의한 Route로만 이동가능하다.  

따라서 

```js
const MainNavigation = createStackNavigator(
  {
    TabNavigation,
    PhotoNavigation,
    MessagesNavigation
  },
  {
    headerMode: "none"
  }
);

```

위에 존재하는 Navigation이 아니면 이동할 수 없음. ( 정해진 Router가 아닌 경우 이동할 수 없는 사황과 비슷함. )

#### `photoNavigation`으로 이동하면 어느 화면을 보여주는가 ?

`PhotoNavigation.js`

```js
import {
  createStackNavigator,
  createMaterialTopTabNavigator
} from "react-navigation";
import TakePhoto from "../screens/Photo/TakePhoto";
import SelectPhoto from "../screens/Photo/SelectPhoto";
import UpLoad from "../screens/Photo/UpLoad";
const PhotoTabs = createMaterialTopTabNavigator(
  {
    TakePhoto,
    SelectPhoto
  },
  {
    tabBarPosition: "bottom"
  }
);

export default createStackNavigator({
  PhotoTabs,
  UpLoad
});

```

`PhotoNavigation.js`에서 initailScreen이 TakePhoto이기 때문에 TakePhoto를 보여줌. 즉, 해당 Navigation으로 이동하면 그 navigation이 보여주기로한 screen을 보여주는 것임. 