---
layout: post
title:  "[ReactNative] 컴포넌트 정리  "
date:   2019-06-09 01:16:00
author: 한만섭
categories: reactnative
tags: reactnative component
---

> ## TextInput 
- value : 입력하는 값을 담는 state혹은 prop 넣기
- placeholderTextColor : 적혀있는 글씨 색 정하기 
- onChangeText : 텍스트를 입력할 경우 호출되는 함수 적기 
- returnKeyType : 키패드의 enter부분을 무엇으로할것인지 ? done = 제출
- atuoCorrect : 자동완성 꺼놓는게 좋음
- onSubmitEditing : 제출을 했을 경우 호출되는 함수를 적으면 됨. 

> ## ScrollView
- 스크롤이 가능한 View 


> ## StyleSheet
html에서 style={}와 동일   
```
StyleSheet.create({})
```


> ## AppLoading
데이터 로딩시 보여지는 화면  


> ## TouchableOpacity
태그의 터치 이벤트를 사용할 수 있는 컴포넌트 
- inPressOut 을 통해서 눌렀을 때 함수를 지정할 수 있음.  


> ## Dimensions
보여지는 화면의 사이즈를 불러오기 위한 component
```
const {height,width} = Dimensions.get("window");
```
