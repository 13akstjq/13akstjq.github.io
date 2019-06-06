---
layout: post
title:  "[ReactNative] expo 프로젝트 생성 및 Git 설정 "
date:   2019-06-07 01:30:00
author: 한만섭
categories: reactnative
tags: reactnative expo git 
---



> ## 시작하기전에...
어제 expo 프로젝트가 갑자기 뻑나는 바람에 처음부터 다시 프로젝트 만들며 공부했는데 이번엔 진짜 잘 정리해서 적어놔야겠다.  
분명 몇일 지나면 다시 까먹을테니....  

　  
   
> ### 1.expo 프로젝트 생성 
expo 프로젝트를 생성하기 위해서는 커맨드 창을 켜야한다. 그리고 프로젝트를 만들 경로로 이동해야한다.  
이동하는 방법 =  `cd 파일경로`  
프로젝트를 만들 경로에 왔다면 다음과 같은 커맨드를 입력한다. (node, create-react-native-app, expo가 전부 설치되어있는 상태입니다. )  
```
expo init todo-app
```
이것저것 설치가 완료될 것이다.  

> ### 2. expo 프로젝트와 Git 연동해놓기  
프로젝트를 시작할때 미리 Git 과 연동해놓고 commit과 push를 연습해보는 것이 Git을 잘모르는 저에게는 도움이 될 것 같아서 해보았습니다.  
우선 Git Repository를 만들어야 한다.  ( 이것은 생략) 
만들었다면 Git Bash 를 켜서 명령을 해야한다.  
0. 우선 git Bash에서 expo 프로젝트 경로로 이동한다. (`cd`)
1. 방금 만든 repository를 원격 저장소에 remote한다. (`remote`)
2. origin master를 pull 해서 가져온다. (`pull`)
3. 만들었던 expo project를 git stage에 올린다. (`add`)
4. staged된 파일을 commit 한다. (`commit`)
5. commit된 내용들을 원격 저장소에 push한다. (`push`)  
위와 같은 형태로 진행한다.

0. expo프로젝트인 todo-app으로 이동
```
$ cd ~/Desktop/rn/todo-app
```

1. 원격저장소에 remote하기 
```
$ git remote add origin https://github.com/13akstjq/todo-app
```

2. 원격 저장소에 있는 파일들 pull로 땡겨오기 
```
$ git pull origin master
```

3. expo프로젝트 stage에 add하기 (.는 파일전체)
```
$ git add .
```

4. stage에 있는 파일 commit하기 (-m "커밋메세지")
```
$ git commit -m "initial commit"
```

5.원격 저장소에 push 해야하므로 origin master에 푸쉬하기 
```
$ git push origin master
```


이렇게 진행하면 github에서 레퍼지토리에 exproject가 추가된 모습을 확인할 수 있습니다.  

　  
   
 
> ### 가상 emul로 expo project 실행해보기 
1. 안드로이드 스튜디오를 킨다.  
2. AVD 매니져를 통해 가상기기를 킨다.  
3. 안드로이드 스튜디오의 터미널을 통해 expo프로젝트 경로로 이동한다.  
4. `expo start --android`를 통해 실행시킨다.  

위와 같이 하면 오래기다리다보면 가상 기기의 expo 를 통해 app이 실행될 것입니다. 
