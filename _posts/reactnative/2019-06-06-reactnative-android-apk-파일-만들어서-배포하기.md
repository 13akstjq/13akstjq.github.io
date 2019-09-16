---
layout: post
title:  "[ReactNative] ReactNative로 만든 앱 apk 만들어 실행해보기"
date:   2019-06-06 21:55:00
author: 한만섭
categories: reactnative
tags: reactnative android apk
---

* TOC
{:toc}
## 정리할 내용
ios배포는 제가 하지 못하는 관계로 안드로이드만 해보려고 합니다.  
실제 배포는 앱스토어의 인증체계에 대해 공부한 후에 해보도록 하겠습니다.  

　  

   

### 1. apk파일 생성하기 
안드로이드에서는 .apk 파일이 필요하기 때문에 apk파일을 만들어야합니다. 만드는 코드부터 얘기하자면  

```
expo build:android
```
위의 코드로 apk를 생성할 수 있는데 하게되면 아래와 같은 에러를 띄우게 됩니다.  
```
standalone-apps/#2-configure-appjson
```

### 2. app.json 수정하기 
위의 에러코드를 보니 app.json을 수정하라는 것 같습니다. app.json파일에 가서 밑에있는 부분을 이렇게 바꿔줍니다. 

```
"android": {
      "package": "com.yourcompany.yourappname"
    }
```

### 3. build 하기 
이제는 빌드를 할 수 있는 상황이 되었습니다. 

```
expo build:android
```

위와 같이 코드를 입력하면 아래와 같이 expo에게 인증을 맡길것인지 물어보는데 저는 1을 선택했습니다.
```
[exp] Making sure project is set up correctly...
[exp] Your project looks good!
[exp] Checking if current build exists...
[exp] No currently active or previous builds for this project.
? How would you like to upload your credentials?
 (Use arrow keys)
❯ Expo handles all credentials, you can still provide overrides
  I will provide all the credentials and files needed, Expo does no validation
```

선택하고나면 오랜시간동안 vscode에서 이것저것 합니다.  
build가 끝나고 나면 아래와 같이 나옵니다. 
```
√ Build finished.
Successfully built standalone app: apk다운받을 주소
```

### 4. 앱 저장및 실행
주소를 통해서 들어가면 apk파일을 받게 되는데 apk파일을 받아서 이메일을 통해 기기에 보내 설치하면 완성입니다. 


[참고 사이트](https://anpigon.github.io/blog/kr/@anpigon/react-native-todo-5-1544624822669/)
