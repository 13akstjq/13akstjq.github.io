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

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<!-- n잡 블로그 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1235773082"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

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

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<!-- n잡 블로그 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1235773082"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

***



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



***



### 4. cannot read property 'bind' of undefined

위 에러는 아래처럼 써야하는데

```react
import {gql} from 'apollo-boost';
```

아래처럼 써서 발생한 에러입니다.

```react
import gql from 'apoolo-boost';
```



***



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



***



### 6. navigationOptions

![1565018624154](../../../../assets/image/1565018624154.png)

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<!-- n잡 블로그 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1235773082"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

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



***



### 7. 문법 실수 



- `LinearGradient` 를 `LenriearGradient`라고 했는데 직접적인 error log가 뜨지 않아서 찾는데 어려웠음...  
  ![image](https://user-images.githubusercontent.com/46010705/59009675-8f61b680-8869-11e9-9d14-1fc986c091f7.png)  

  <script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
  <!-- n잡 블로그 수평 -->
  <ins class="adsbygoogle"
       style="display:block"
       data-ad-client="ca-pub-4877378276818686"
       data-ad-slot="1235773082"
       data-ad-format="auto"
       data-full-width-responsive="true"></ins>
  <script>
       (adsbygoogle = window.adsbygoogle || []).push({});
  </script>
  
- const {isLoaded} = this.state;  
  render함수 내에서 const로 state의 변수를 사용할 때 {}사이에 넣어주어야한다. ( react,rn에서의 {}의 의미에 대해서 좀 더 이해할 필요있음)
  
- `Ionicons` 를 `ionIcons` 낙타형식으로 써서 에러...
  [expo icon 사이트](https://expo.github.io/vector-icons/)
  
- `export default (function or class Name)`
  따로 파일형태로 만든 class 혹은 function은 export를 꼭 해주어야한다.  
  error log에서 export를 잊은거 아니냐고 알려주니 확인해볼 것.

```
C:\Users\mshan\Desktop\rn\weather-app\node_modules\react-native\Libraries\Core\ExceptionsManager.js:84 Warning: React.createElement: type is invalid -- expected a string (for built-in components) or a class/function (for composite components) but got: object. You likely forgot to export your component from the file it's defined in, or you might have mixed up default and named imports.
```

- propTypes 대소문자 틀림   
  functionName.`P`ropTypes라고 하면 노란 창으로 알려주니까 읽고 수정할 것 

```
Weather.propTypes={
    temp : PropTypes.number.isRequired
}
```

import 방법 

```
import PropTypes from "prop-types";
```

- render내에서 state사용시 주의
  component의 `state`를 render()에서 사용하기 위해서는 아래와 같이 render내에서 this.state를 변수로 갖고 있어야한다. 

```
const {newToDo} = this.state;
```



***



### 8. textline 버그 

자꾸 TextInput에서 returnkeytype을 done으로 해도 적용이 되지 않길래 봤더니 multiline 속성을 사용하고 있으면  
returnkeytype이 적용되지 않는 버그가 있었다. 안드로이드에서만 존재하는 것 같다.  
임시방편으로 일단 multiline은 사용하지 않은채 개발했다. 



------



### 9. TouchableOpacity

커스텀 버튼을 만들기 위해 이 componente를 사용하면 `Text`와 같은 component도 클릭 이벤트를 사용할 수 있다.   



***



### 10. expo 설치시 발생하는 에러 

설치할 파일 경로로 이동 후 

```
expo init "프로젝트 명"
```

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<!-- n잡 블로그 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1235773082"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

expo 프로젝트를 생성했는데 `react native is not install`이라는 메세지를 볼수도 있다. 
정확한 이유는 없지만 다시 설치만 해주면 정상적으로 동작하는 이슈이다. 

- 해결방법

pakage.json에 없는 dependency를 추가해주면 되는데, npm이 깔려있기 때문에 명령어 하나면 쳐주면 된다.  

```
npm i
```

위 명령어는 자동으로 필요한 `dependency`를 `pakage.json`에 설치해주는 명령어이다. 



### 11. 안드로이드 geolocation timeout 이슈 

안드로이드에서만 발생하는 이슈인데 인터넷에 있는 글을 통해서 해결은 했지만 정확한 이유는 알지 못하므로 추후에 알아보고 내용을 업데이트 해야겠다.  

- timeout 이슈 

안드로이드에서 현재 위치정보를 불러오는 navigator.geolocation을 사용하면 반응이 없는 이슈가 있다.  
아래와 같이 코드를 작성하면 응답이 없음... ios에서는 발생하지않는 이슈인데 왜인지 모르겠음. 

```
navigator.geolocation.getCurrentPosition(
      function(position) {
        console.log(position);
      }, 
      function(error) {
        console.log(error);
      }
    );
```

- 이슈 해결 방법

아래와 같이 작성하면 안드로이드 스튜디오의 콘솔에서 위치정보를 불러오는 것을 확인할 수 있다.  

```
  componentDidMount(){ 
    console.log("didMount");
    navigator.geolocation.getCurrentPosition((position) => {
      console.log(position);
  
  }, (err) => {
      console.log(err);
  
  }, { enableHighAccuracy: true, timeout: 2000, maximumAge: 3600000 })
    
  }
```

- 안드로이드 스튜디오 콘솔 화면  

```
didMount
Object {
  "coords": Object {
    "accuracy": 20,
    "altitude": 5,
    "heading": 0,
    "latitude": 37.4219983,
    "longitude": -122.084,
    "speed": 0,
  },
  "mocked": false,
  "timestamp": 1559792054000,
}

```



- 생각

highAccurancy를 해줘야 한다는 글은 많이봤지만 이유를 아직 모르기 때문에 알아봐야 될 것 같고, 이것은 약간의 꼼수라고 한다. 

```
{ enableHighAccuracy: true, timeout: 2000, maximumAge: 3600000 }
```

[참고 사이트](https://medium.com/@steve.mu.dev/solve-location-request-timed-out-problem-when-using-geolocation-in-react-native-3674e08f3688)

