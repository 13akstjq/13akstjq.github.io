---
layout: post
title: "[ReactNative] 인스타그램클론 이슈 정리 "
date: 2019-07-28-22:48:00
author: 한만섭
categories: reactnative
tags: reactnative dev issue
---

- TOC
  
  {:toc}

---

### 1. 하위 컴포넌트에게 함수 넘겨주기

```js
const AuthHome = ({ navigation: { navigate } }) => {
  console.log(constants.width);
  return (
    <View>
      <Logo resizeMode={"contain"} source={require("../../assets/logo.png")} />
      <AuthButton
        text={"Create New Account"}
        onPress={() => navigate("Signup")}
      />
      <Touchable onPress={() => navigate("Login")}>
        <LoginButton>
          <LoginText>Log in</LoginText>
        </LoginButton>
      </Touchable>
    </View>
  );
};
```

`onPress`를 넘겨줄 때 아래와 같이 넘겨주는 게 아니라 콜백방식으로 넘겨줘야함.

```js
onPress={navigate("Signup")}
```

옳은 방법

```js
onPress={() => navigate("Signup")}
```

---

### 2. Dimension.get("screen")

화면의 크기를 구할 때 사용하는 방식

아래와 같이 사용하면 앱이 꺼지는 현상 발생

```js
import { Dimensions } from "react-native";

const { width, height } = Dimensions;

export default { width, height };
```

- 옳은 방법

```js
import { Dimensions } from "react-native";

const { width, height } = Dimensions.get("screen");

export default { width, height };
```

---

### 3. TextInput OnChangeText

```js
const AuthInput = ({ value, onChange, placeholder, keyboardType }) => {
  return (
    <InputContainer>
      <Input
        value={value}
        onChangeText={onChange}
        placeholder={placeholder}
        keyboardType={keyboardType}
      />
    </InputContainer>
  );
};
```

Reactnative의 onchange함수는 `onChangeText`입니다..

[TextInput 공식 DOC](https://facebook.github.io/react-native/docs/textinput.html)

### 4. cannot read property 'bind' of undefined

위 에러는 아래처럼 써야하는데

```react
import {gql} from 'apollo-boost';
```

아래처럼 써서 발생한 에러입니다.

```react
import gql from 'apoolo-boost';
```

### 5. backend에 보내야 하기 때문에 `setContext`

```js
 //create apolloClient
      const client = new ApolloClient({
        cache,
        request: async operation => {
          const token = await AsyncStorage.getItem("jwt");
          return operation.setContext({
            headers: { Authorization: `Bearer ${token}` }
          });
        },
        ...apolloClientOptions
      });

```

### 6. navigationOptions

![1565018624154](../../../../assets/image/1565018624154.png)

위에 보이는 navigationOptions는 navigator의 안에 있는 screen 에 넣는 것을 의미한다.

```js
const PhotoTabs = createMaterialTopTabNavigator(
  {
    Take: {
      screen: TakePhoto,
      navigationOptions: {
        tabBarLabel: "Take"
      }
    },
    Select: {
      screen: SelectPhoto,
      navigationOptions: {
        tabBarLabel: "Select"
      }
    }
  },
  {
    tabBarPosition: "bottom",
    tabBarOptions: {
      backgroundColor: Theme.lightGreyColor,
      activeTintColor: Theme.blackColor,
      inactiveTintColor: Theme.darkGreyColor,
      indicatorStyle: {
        backgroundColor: Theme.blackColor
      },
      tabStyle: {},
      style: {
        marginBottom: 10,
        backgroundColor: Theme.lightGreyColor
      }
    }
  }
);
```
