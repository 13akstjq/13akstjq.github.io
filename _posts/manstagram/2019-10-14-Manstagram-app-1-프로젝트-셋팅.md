---
layout: post
title: "Manstagram - app.1 - ReactNative 개발을 위한 설정"
date: 2019-10-14-15:23:00
author: 한만섭
categories: manstagram
tags: React ReactHooks ReactNative Graphql Prisma Apollo
---



* TOC
{:toc}
![image](https://user-images.githubusercontent.com/46010705/66889207-81dcea00-f01c-11e9-850c-5534ab9ea5d1.png)

ReactNativee + Expo 프로젝트를 어떻게 설정했는지 정리한 글입니다. 

***

### 1. Node.js npm 설치하기

`reactnative expo`를 설치하기 위해서는 npm 을 사용해야하는데 npm은 node.js를 설치해야 사용가능하다. 그렇기 때문에 node.js를 설치해보려 한다.  

[node.js 설치](https://nodejs.org/ko/) 페이지에서 가장 최신 버전의 node.js를 다운 받으면된다. 설치할때는 특별히 설정해야할 것이 없기 때문에
다음다음 넘기면 된다. 설치 후에 vscode를 재실행하면 적용이 된다.  

> ### npm 버전 확인하기 

```
npm -v
```



------



### 2. Create React Native App 설치하기  

쉬운 앱 개발을 위해 필수인 RN을 깔 차례다. 앱스토어와 비슷한 npm을 통해서 깔아보려고 한다.  

1. Create-react-native-app install 
   콘솔창에 다음과 같이 입력 

```
$ npm install -g create-react-native-app
```

2. 개발할 App 생성 (생성하기 전에는 cd를 통해서 생성할 레퍼지토리로 이동해놓기)

```
$ create-react-native-app my-app
```

3. App으로 이동하기 

```
$ cd my-app
$ npm start  
```



이제 npm을 통해서 create-react-native-app 까지 설치를 완료했다. 이제는 RN의 능력을 더욱 극대화 해줄 Expo를 설치하려고 한다. 

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

------



### 3. Expo 설치 

RN의 능력을 더욱 빛나게 해줄 Expo를 설치하려고 한다. Expo는 Reactnative로 수정한 프로젝트를 hot-code로 적용시킬 수 있다. 

콘솔에 다음과 같이 입력한다. 

```
$ npm install -g expo-cli
```

전에 만든 App을 build하는 방법

```
$ expo start
$ expo start --android
```



------



### 4. AndroidStudio 환경변수 설정 

AndroidStudio를 사용하는 이유는 Expo를 사용하여 실물 폰과 연동해서 사용하려고 했지만, 아무리 구글링을 해서 EXPO의 QR코드를 찍어봐도 
`timeout`이 발생해서 안드로이드 스튜디오의 `ADB Manager`를 사용해 가상으로 돌리기로 했다.  
`ADB' (Android Debug Bridge) : 에뮬레이터 인스턴스나 연결된 Android 기기와 통신할 수 있는 명령줄 도구.  

출처: https://wangin9.tistory.com/entry/wls-expo-cli-설치 [잉구블로그]

Expo를 사용해서 하면 분명 편하겠지만 어쩔 수 없었음....  

1. 환경변수 설정 
   [EXPO에서 제공하는 안드로이드 스튜디오 설정] (https://docs.expo.io/versions/latest/workflow/android-studio-emulator/)

- 윈도우버튼 마우스 오른쪽 버튼으로 시스템 실행 > 고급시스템 설정 > 환경 변수
- 혹은 윈도우 검색에 '시스템 환경변수 편집' 검색해서 환경변수 제어판 실행
- 환경 변수 : 시스템 변수 -> 새로만들기 
  변수 이름에 Android-SDK, 변수 값에 본인의 Android SDK 설치 경로 입력
  ![image](img/58800274-5eec0380-8642-11e9-8cdb-3b102da311ab.png)
- 시스템변수 Path 에 JAVA JDK 폴더 경로 추가
  Android SDK 가 설치된 platform-tools 폴더 내에 adb.exe 파일이 존재한다.
  (\Android\Sdk\platform-tools)
  ![image](img/58800326-85aa3a00-8642-11e9-95ee-55164fd61205.png)
- Android-SDK 변수명과 platform-tools 폴더 경로를 추가하여 PATH에 환경 변수를 추가해준다.
  %Android-SDK%\platform-tools
  ![image](img/58800351-978bdd00-8642-11e9-9ce6-5273ef093aa1.png)
  실제 파일 경로를 넣어주면 된다. 

2. Android SDK 환경변수 설정 확인

- Ctrl + R 눌러 실행 창에서 cmd 로 command 창 실행
- adb sherll 입력해서 아래와 같이 device 가 연결 된다면 완료
  ![image](img/58800425-c30ec780-8642-11e9-94aa-a5446b20ae6a.png)

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

#### 4.1 AndroidStudio SDK 및 ADB 설정 

- SDK Manager를 선택해준다. 
  ![image](img/58801221-dfabff00-8644-11e9-85e4-433d81f8a482.png)
- SDK Platform 설정
  ![image](https://user-images.githubusercontent.com/46010705/58801294-0f5b0700-8645-11e9-8f78-cf05ed86a137.png)
  빨간 박스 두개 다 해줘야 한다고 했는데 하나만 해도 되더라.
- SDK Tools
  ![image](img/58801353-403b3c00-8645-11e9-9218-b5d69a94cc90.png)
  Android SDK build tools 을 다운 받는다.  
- Usb Debugging 허용
  ![image](https://user-images.githubusercontent.com/46010705/58804313-8a73eb80-864c-11e9-8311-f6d4c19291ed.png)
  `android:debuggable="true"`
  다음 코드를 넣어주면 실제 핸드폰 개발자 환경에서 usbDebugging을 풀어주는 것과 같다. 

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

#### 4.2 emulator 만들기 



#### 4.3 19000 , 19001 포트 인바운드 허용해주기 

- 방화벽 들어가기 
  ![image](https://user-images.githubusercontent.com/46010705/58807833-c9f20600-8653-11e9-9e8a-b92c0c4af154.png)  
- 규칙 만들어주기 
  ![image](https://user-images.githubusercontent.com/46010705/58807943-032a7600-8654-11e9-8354-38147281a7aa.png)



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

#### 4.4Expo 핸드폰 이슈 

QR 코드 이용해서 디바이스에서 실행시,

network response timed out 

발생.

1) 디바이스와 같은 wi-fi 사용중인지 확인

2) wi-fi  public 에서 private 로 변경

3) 방화벽 인바인드 규칙에 19000 과 19001 포트 규칙 추가.

그래도 안된다면,

https://github.com/react-community/create-react-native-app/issues/144#issuecomment-308394689   확인.

IPv4 주소 값으로 세팅.

 set REACT_NATIVE_PACKAGER_HOSTNAME=my-custom-ip-address

unix ) set 대신 export



------



### 5. 프로젝트 실행시키기 

이제 모든 설정을 끝마쳤기 때문에 app을 실행해보려고한다. 

![image](https://user-images.githubusercontent.com/46010705/58806968-310ebb00-8652-11e9-82bc-68fec9423e13.png)
expo로 android를 실행시키는 코드

```
expo start --android
```

성공했다면 이런 내용이 나온다.  
![image](https://user-images.githubusercontent.com/46010705/58807203-9a8ec980-8652-11e9-90cf-1636f2958b5c.png)

expo에는 이런 화면이 나온다.
![image](https://user-images.githubusercontent.com/46010705/58807308-c316c380-8652-11e9-805c-acce1be7b91f.png)

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

핸드폰 화면

![image](https://user-images.githubusercontent.com/46010705/58807382-eb062700-8652-11e9-8fd9-b775e144eec1.png)