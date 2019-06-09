---
layout: post
title:  "[ReactNative] react/reactnative에서 외부 폰트 적용하기   "
date:   2019-06-09 16:21:00
author: 한만섭
categories: reactnative
tags: reactnative react font
---



> ## 시작하기 전에...
react native를 개발하면서 필요한걸 하나씩 추가할 때마다 모든게 새로 배워야하는 것이기 때문에 해보고 싶은게 있다면 뭐든 
해보고 넘어가는 것이 좋은것 같다.  

font적용도 사실 할 생각은 없고 쉽겠지..? 라는 생각으로 나중에 하려다가 한번 해봣는데, 상대경로 및 공식 Document를 찾아보는 방법이 필요한 것 같다.  
역시 영어를 잘하면 개발할때 남들보다 빠르게 좋은 정보를 얻을 수 있는 것 같다.  

> ### 외부 폰트 적용하기 
제가 적용할 폰트는 `배달의 민족` 폰트인 `배민 한나 프로체`입니다. 폰트를 적용하는 방법에는 `CDN`을 사용하는 방법과 `직접 local 파일을 loading하는 것`
인데, 저는 후자만 공부해보도록 하겠습니다. 이유는 CDN은 편하지만, 매번 CDN의 링크에게 요청을 보내 받는 형식으로 알고 있습니다. 이게 속도의 저하를 불러
일으킨다고... 확실하진 않지만 하는김에 local에 파일 저장하는 방식으로 해봤습니다.  

> ## 1. 폰트파일 저장하기 
외부폰트를 local에서 시용하고 싶다면 당연히 폰트 파일이 있어야 겠죠. 우선 사용한 폰트 파일을 다운 받습니다. 저는 앞서 말한대로 배민폰트를 사용하겠습니다.  
[배민 폰트 사이트](https://www.woowahan.com/#/fonts) 에서 window 버전을 받았습니다.  

> ## 2. 프로젝트에 위치시키기 
폰트를 다운로드 받았다면 프로젝트에 위치시켜야겠죠. reactnative 프로젝트를 기준으로 말씀드리겠습니다.  
`assets/fonts/`에 다운 받은 `.ttf`형식의 파일을 위치시키겠습니다. 
![image](https://user-images.githubusercontent.com/46010705/59156309-fd111b00-8ad3-11e9-9c02-a111fddf70e8.png){: width="100" height="100"}  



> ## 3. font 파일 경로 pakage.json에 추가시키기 
아래 코드를 `pakage.json`에 추가시켜 줍니다.  
```
"rnpm": {
    "assets": [
      "./assets/fonts"
    ]
  }
```  

　  
   
> ## 4. expo-font install 하기 
저는 expo를 이용해 프로젝트를 제작했습니다. 그래서 expo-font를 이용해 외부 폰트를 불러올 것입니다. [expo 공식 문서](https://docs.expo.io/versions/latest/sdk/font/#returns)  
우선 npm으로 expo-font를 설치해줍니다.  
```
npm install expo-font
```
인스톨이 완료되었다면 사용할 파일에 expo-font를 import해야합니다. 
```
import * as Font from 'expo-font';
```

import까지 다했기 때문에, 이제 외부폰트를 로드하는 작업만 하면 사용할 수 있습니다. 

```
Font.loadAsync({
  Montserrat: require('./assets/fonts/Montserrat.ttf'),
  'Montserrat-SemiBold': require('./assets/fonts/Montserrat-SemiBold.ttf'),
});
```
왼쪽에 폰트를 사용할 이름을 적고 `require`을 local경로에 맞춰서 해주면 font를 사용할 수 있습니다.  

여기서 저는 파일 경로 때문에 시간을 좀 썼네요,,,,  
![image](https://user-images.githubusercontent.com/46010705/59156360-f2a35100-8ad4-11e9-8d1f-dc713a7993cf.png)
<image src = "https://user-images.githubusercontent.com/46010705/59156360-f2a35100-8ad4-11e9-8d1f-dc713a7993cf.png" width : 50%>

저는 index.js에서 assets/fonts/에 있는 배민폰트를 불러야했는데 상대 경로를 표시하는 것 때문에 동작을 안했습니다.  
```
require('../../assets/fonts/BMHANNAPro.ttf')});
```
`../`가 상위경로로 이동하는 것이기 떄문에 상위 경로로 2번가고 난 후에 assets/fonts를 했어야 했습니다.  
하시는 분들도 경로만 조심하시면 별 문제 없이 작동할것 같습니다~~

> ### 적용전 화면 
![image](https://user-images.githubusercontent.com/46010705/59156393-67768b00-8ad5-11e9-82d7-af168d84bc2f.png){: width="100" height="100"}


> ### 적용후 화면
![image](https://user-images.githubusercontent.com/46010705/59156386-4d3cad00-8ad5-11e9-870f-cca28fec6017.png){: width="100" height="100"}

