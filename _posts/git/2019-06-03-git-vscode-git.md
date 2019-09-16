---
layout: post
title:  "[Git] VS code에 GitHub 연동하기 "
date:   2019-06-03 
author: 한만섭
categories: git
tags: git vscode
---

* TOC
{:toc}
## 시작하기 전에...
개발하면서 느낀거 진짜 개발하는 것보다 개발하기전에 `개발환경` `git` 설정하는게 진짜 힘든 것 같다. Git은 미루다 미루다 이제 하는거지만 
Git command , eclipse에서 git 연동하는 것도 다시 한번 정리 해놔야 할것 같다. 

## 1. VScode에 Git 연동하기 

### 1.1 Git 설치하기
우선 Git을 연결하려면 `Git`을 깔아야한다. 여기서 말하는 Git은 `GitBash`를 말한다. [Git 설치하기](https://git-scm.com/download)  

### 1.2 Git id,email설정 

```
$ git config --global user.name "13akstjq"
$ git config --global user.email "13akstjq@naver.com"
```
그 다음에 vscode를 재실행하면 vscode가 알아서 git을 잡아준다.  

## 2. 기존의 프로젝트를 원격저장소와 연결하기 

1. 만들었던 프로젝트 파일을 연다.  
2. git workspace를 지정해준다.(만들었던 프로젝트 파일) 
3. 방금 만든 비어있는 원격 저장소와 프로젝트를 remote해준다. 
```git
$ git remote add origin "레퍼지토리 URL"
```
4. 여기까지 하면 로컬에만 들어가 있기 때문에 push 를 해줘서 원격 저장소까지 밀어준다.

## 3. gh-pages branch만들기 

배포하기 위해서는 master branch가 아닌 `gh-pages`branch가 필요하다.  

### 4. 원격 저장소에 gh-pages branch 만들기

```git
$ cd "현재 프로젝트 경로"
$ git branch gh-pages
$ git push origin gh-pages
```
원격 저장소에 branch를 만들었다면, 배포할 기본 준비가 끝난 것이다.  

## 5. 최적화된 build 파일 deploy하기 

### 5.1 프로젝트파일 build하기 

```bash
npm run build
```

### 5.2 pakage.json에 홈페이지 추가 

```bash
"homepage": "http://13akstjq.github.io/movie-app"
```

### 5.3 gh-pages -d build 설치하기 

```bash
gh-pages -d build
```

### 5.4 시키는대로 코드 추가 하기 

```json
"scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject",
    "predeploy": "npm run build",
    "deploy": "gh-pages -d build"
  },
```

### 배포하기 

```bash
npm run deploy
```



바탕화면 경로 
~/Desktop/vs-git
