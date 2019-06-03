---
layout: post
title:  "[ReactNative] 4.AndroidStudio 환경변수 설정  "
date:   2019-06-03 22:13:00
author: 한만섭
categories: reactnative
tags: reactnative androidstudio
---

> ## Native EXPO 연동하기 
1. Node.js npm 설치하기 
2. React-Native-App 설치하기 
3. Expo 설치 및 프로젝트 생성
4. `AndroidStudio 환경변수 설정`
5. ADB Manager를 이용해 실행시키기  

> ### AndroidStudio 환경변수 설정 


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
![image](https://user-images.githubusercontent.com/46010705/58800274-5eec0380-8642-11e9-8cdb-3b102da311ab.png)

- 시스템변수 Path 에 JAVA JDK 폴더 경로 추가
  Android SDK 가 설치된 platform-tools 폴더 내에 adb.exe 파일이 존재한다.
  (\Android\Sdk\platform-tools)
![image](https://user-images.githubusercontent.com/46010705/58800326-85aa3a00-8642-11e9-95ee-55164fd61205.png)
 - Android-SDK 변수명과 platform-tools 폴더 경로를 추가하여 PATH에 환경 변수를 추가해준다.
     %Android-SDK%\platform-tools
![image](https://user-images.githubusercontent.com/46010705/58800351-978bdd00-8642-11e9-9ce6-5273ef093aa1.png)
실제 파일 경로를 넣어주면 된다. 

2. Android SDK 환경변수 설정 확인

- Ctrl + R 눌러 실행 창에서 cmd 로 command 창 실행

- adb sherll 입력해서 아래와 같이 device 가 연결 된다면 완료
![image](https://user-images.githubusercontent.com/46010705/58800425-c30ec780-8642-11e9-94aa-a5446b20ae6a.png)



> ## AndroidStudio SDK 및 ADB 설정 
- SDK Manager를 선택해준다. 
![image](https://user-images.githubusercontent.com/46010705/58801221-dfabff00-8644-11e9-85e4-433d81f8a482.png)

- SDK Platform 설정
![image](https://user-images.githubusercontent.com/46010705/58801294-0f5b0700-8645-11e9-8f78-cf05ed86a137.png)
빨간 박스 두개 다 해줘야 한다고 했는데 하나만 해도 되더라.

- SDK Tools
![image](https://user-images.githubusercontent.com/46010705/58801353-403b3c00-8645-11e9-9218-b5d69a94cc90.png)
Android SDK build tools 을 다운 받는다.  


- Usb Debugging 허용
![image](https://user-images.githubusercontent.com/46010705/58804313-8a73eb80-864c-11e9-8311-f6d4c19291ed.png)
`android:debuggable="true"`
다음 코드를 넣어주면 실제 핸드폰 개발자 환경에서 usbDebugging을 풀어주는 것과 같다. 


> ## emulator 만들기 


> ## 19000 , 19001 포트 인바운드 허용해주기 
- 방화벽 들어가기 
![image](https://user-images.githubusercontent.com/46010705/58807833-c9f20600-8653-11e9-9e8a-b92c0c4af154.png)  

- 규칙 만들어주기 
![image](https://user-images.githubusercontent.com/46010705/58807943-032a7600-8654-11e9-8354-38147281a7aa.png)



> ## Expo 핸드폰 이슈 
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

