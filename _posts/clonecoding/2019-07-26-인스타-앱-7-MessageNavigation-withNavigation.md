---
layout: post
title: "[인스타클론코딩] [App].7- MessagesNavigation, withNavigation 정리"
date: 2019-07-28-19:03:00
author: 한만섭
categories: clonecoding
tags: react instagram withNavigation, MessageNavigation
---





















> ### 실수한 점 

* `export default () => `

```js
import { createStackNavigator } from "react-navigation";
import Messages from "../screens/Message/Messages";
import Message from "../screens/Message/Message";

export default () =>
  createStackNavigator({
    Messages,
    Message
  });

```

위와 같이 작성하는 바람에 아래와 같은 에러가 발생했습니다. 에러는 React의 Child로는 `function`이 적당하지 않다고 얘기합니다. 에러만 제대로 읽었어도 해결했을텐데 또 삽질을.....

```
Functions are not valid as a React child. This may happen if you return a Component instead of <Component /> from render. Or maybe you meant to call this function rather than return it.%s, 
```

* 해결 방법 

  ```js
  import { createStackNavigator } from "react-navigation";
  import Messages from "../screens/Message/Messages";
  import Message from "../screens/Message/Message";
  
  export default createStackNavigator({
    Messages,
    Message
  });
  
  ```

  위와 같이 `createStackNavigator`를 바로 export default 해주게 되면 함수를 return하던 에러가 사라지게 됩니다.  

  

