---
layout: post
title:  "[API] ReactNative에서 openWeathermap API사용하기 "
date:   2019-06-06 17:46:00
author: 한만섭
categories: api
tags: reactnative openweathermap
---

> ## 날씨 API사용하기
ReactNative에서 날씨를 알려주는 App을 만들기 위해 날씨 API를 필요로 했다.  
날씨 정보를 제공하는 [OpenWeatherMap](https://openweathermap.org/)을 사용했다. 
현재 위치의 경도와 위도를 기반으로 하기 때문에 혹시 현재 위치정보를 불러오지 못했다면 [ReactNative에서 현재위치 불러오기 ](https://13akstjq.github.io/reactnative/2019/06/06/reactnative-geolocation-solution.html)를 보고오는 것을 추천한다. 

> ## openWeatherMap API사용방법

1. API KEY 발급 받기
무료로 제공하기 때문에 회원가입을 통해서 API-KEY를 제공한다.  
나는 회원가입을 이미 했기 때문에 아래와 같은 화면이 뜬다. 회원가입을 한 후에 `API KEYS`로 들어간다.
거기서 APIKEY를 복사한다.  (회원가입을 하고 10분뒤에 APIKEY사용 가능)
![image](https://user-images.githubusercontent.com/46010705/59019640-65b58900-8883-11e9-827e-b23feec07052.png)


2. API DOC 읽기  
나는 위도와 경도로 날씨를 불러올 것이기 때문에 아래와 같은 부분의 문서를 참고했다. 각자 가지고 있는 정보를 통해서 호출하면 될 것같다.   
![image](https://user-images.githubusercontent.com/46010705/59019894-d0ff5b00-8883-11e9-8764-75182b88db89.png)

발급 받은 APIKEY를 뒤에 붙혀서 호출하면 된다고 한다. 매우 간단하다. 
```
api.openweathermap.org/data/2.5/weather?lat={lat}&lon={lon}&appid=b6907d289e10d714a6e88b30761fae22
```


3. API 호출하기  
이제는 실제 프로젝트에서 호출을 해보려고 한다.  
아래는 현재 위치의 위도,경로를 파라메터로 갖고있는  _getWeather(lat,lon) 함수이다.  
```
fetch('http://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&APPID={API_KEY}')
```
위의 코드는  인터넷에서 본 코드는 ${}를 통해서 입력을 했지만, 이렇게 할경우 아래와 같은 error 를 발생시킨다. 
```
Object {
  "cod": "400",
  "message": "${lat} is not a float",
}
```

  

그래서 직접 인자로 들어온 `lat` 과 `lon`을 붙혀서 호출하는 방법을 선택했다.  


```
  _getWeather = (lat,lon) =>{
    console.log(lat + lon);
    fetch('http://api.openweathermap.org/data/2.5/weather?lat='+lat+'&lon='+lon+'&APPID='+API_KEY)
    .then(response => response.json())
    .then(json => {
      console.log(json)
    })
  }
```


> ## 마치며...
주워들은 바에 의하면 react는 링크안에 ${}로 넣으면 원래는 되어야 하는데 나는 왜 에러를 발생시키는지 모르겠다. 
