---
layout: post
title:  "[ReactNative] 안드로이드 geolocation timeout 이슈 해결하기 "
date:   2019-06-06 16:35:00
author: 한만섭
categories: reactnative
tags: reactnative navigation geolocation
---

> ## 시작하기전에...  
안드로이드에서만 발생하는 이슈인데 인터넷에 있는 글을 통해서 해결은 했지만 정확한 이유는 알지 못하므로 추후에 알아보고 내용을 업데이트 해야겠다.  

> ### timeout 이슈 
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

> ### 이슈 해결 방법
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

> ### 생각
highAccurancy를 해줘야 한다는 글은 많이봤지만 이유를 아직 모르기 때문에 알아봐야 될 것 같고, 이것은 약간의 꼼수라고 한다. 
```
{ enableHighAccuracy: true, timeout: 2000, maximumAge: 3600000 }
```

[참고 사이트](https://medium.com/@steve.mu.dev/solve-location-request-timed-out-problem-when-using-geolocation-in-react-native-3674e08f3688)
