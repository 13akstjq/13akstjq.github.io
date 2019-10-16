---
layout: post
title: "[ReactNative] react-navigation 정리 "
date: 2019-07-28-16:48:00
author: 한만섭
categories: reactnative
tags: reactnative react-navigation
---

* TOC
{:toc}
## Navigation

`Navigation`이란 `React`에 존재하는 `Router`와 동일한 역할을 합니다. 기존에 나온 RN에서 제공해주는 navigation이 복잡하다고 해서 다양한 곳에서 navigation라이브러리를 만들었고, 그 중 facebook에서 만든 `react-navigation`을 정리해보도록 하겠습니다.

---

### TabNavigation ? StackNavigation ?

이 두개가 어떤 관계가 있는지 정리해보도록 하겠습니다.

- TabNavigation

  ![1564300647676](https://user-images.githubusercontent.com/46010705/66907633-9d5beb00-f044-11e9-896f-98e8d7a3db31.png)

  위와 같은 형태가 tabnavigation이고 HOME 혹은 BOOKMARKS를 눌렀을때 새로운 화면을 보여주는 방식입니다.

- `StackNavigation`

  ![1564300798339](../../../../assets/image/1564300798339.png)

  위 사진 같이 새로운 창이 아닌 기존 화면에 새로운 화면을 쌓아놓는 방식이 `StackNavigation`입니다. 이것을 사용할 경우에 뒤로가기를 할 수 있습니다. 하지만 reloading이 되지는 않습니다.

> ### Navigation 만들기

Router에서는 컴포넌트에 경로를 정해놓으면 사용자가 특정경로를 요청하면 정해놨던 route를 보여주는 방식이었습니다.

Navigation도 React의 Router의 개념과 크게 벗어나지 않습니다.

- StackNavigation

  [react-navigation 공식문서](https://reactnavigation.org/docs/en/stack-navigator.html)

  ![1564301109089](../../../../assets/image/1564301109089.png)

  위에서 보다시피 `createStackNavigator`를 통해서 만들 수 있습니다. 첫번째 인자로는 navigation에 사용될 `Route`를 설정하고, 두번째 인자로는 StackNavigator의 config를 작성하는 곳입니다. 아래 코드는 Home,Login,Signup,Confirm 네개의 화면을 가지고 있는 `AuthNavigation`을 만든 코드입니다. 코드를 보면 각 컴포넌트를 import 하고, `react-navigation`의 `createStackNavigator`를 사용합니다. 그렇다면 `createAppContainer`는 언제 사용하는 것일까요?

```js
import { createStackNavigator, createAppContainer } from "react-navigation";
import AuthHome from "../screens/Auth/AuthHome";
import Signup from "../screens/Auth/Signup";
import Login from "../screens/Auth/Login";
import Confirm from "../screens/Auth/Confirm";
const AuthNavigation = createStackNavigator(
  {
    AuthHome,
    Login,
    Signup,
    Confirm
  },
  {
    headerMode: "none"
  }
);

export default createAppContainer(AuthNavigation);
```

`AuthNavigator`를 만들었다면 `createAppContainer`를 통해서 포장을 해줘야 하는 것입니다.

라이브러리를 만든 개발자가 만든 규칙이라고 볼 수 있습니다. `navigator`를 만들었으면 `createAppContainer`를 감싸줌으로써 navigator로 사용할 수 있는 것이겠죠. 이 방식은 `TabNavigation`을 사용할 때도 동일합니다.

두번째 인자에서 사용한 `headerMode : "none"`은 상단바의 방식을 결정하는 설정입니다. 이외에도 많은 설정이 있으니 [공식문서](https://reactnavigation.org/docs/en/stack-navigator.html)를 참고하시면 될 것 같습니다.

---



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





### Navigation Prop

react에서는 현재 주소를 알기 위해서 Router에서 기본적으로 자식 컴포넌트에게 제공했던 props인 `match,location,`가 있었습니다. 그것이 `react-navigation`에도 존재합니다. 위에서 얘기했다 시피 Router === Navigation인 느낌이기 때문에 Navigation이 갖고 있는 component,navigation에게는 `navigation`이라는 prop을 넘겨줍니다. 그럼 [공식문서](https://reactnavigation.org/docs/en/navigation-prop.html)를 통해서 `navigation`에 어떤 속성이 있는지 알아보겠습니다.

![1564301777057](../../../../assets/image/1564301777057.png)

첫번째에 있는 `navigate`속성을 읽어보면 다른 screen으로 이동할 수 있다고 합니다. 그럼 바로 사용해보도록 하겠습니다.

`AuthHome.js`

```react
import React from "react";
import { TouchableOpacity } from "react-native";
import styled from "styled-components";

const View = styled.View`
  flex: 1;
  justify-content: center;
  align-items: center;
`;

const Text = styled.Text``;

const AuthHome = ({ navigation: { navigate } }) => {
  return (
    <View>
      <TouchableOpacity onPress={() => navigate("Login")}>
        <Text>sign in</Text>
      </TouchableOpacity>
      <TouchableOpacity onPress={() => navigate("Signup")}>
        <Text>sign up</Text>
      </TouchableOpacity>
    </View>
  );
};

export default AuthHome;
```

`AuthHome`이 prop으로 navigation의 함수인 navigate를 받고 있습니다.

그 navigate를 사용해서 `signin`버튼을 누르면 `Login`으로 보내줍니다. 여기나와있는 Login은 아래에 정의되어 있습니다.

`AuthNavigation.js`

```react
import { createStackNavigator, createAppContainer } from "react-navigation";
import AuthHome from "../screens/Auth/AuthHome";
import Signup from "../screens/Auth/Signup";
import Login from "../screens/Auth/Login";
import Confirm from "../screens/Auth/Confirm";
const AuthNavigation = createStackNavigator(
  {
    AuthHome,
    Login,
    Signup,
    Confirm
  },
  {
    headerMode: "none"
  }
);

export default createAppContainer(AuthNavigation);

```

이것으로 보아 prop로 넘겨받은 `navigate`를 사용할 때는 나에게 navigation을 넘겨준 `stackNavigation`에 있는 `Route`에게만 이동이 가능한 것 같습니다.


