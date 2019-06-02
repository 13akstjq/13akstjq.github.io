---
layout: post
title:  "[Git] VS code에 GitHub 연동하기 "
date:   2019-06-03 
author: 한만섭
categories: git
tags: git vscode
---

##시작하기 전에...
개발하면서 느낀거 진짜 개발하는 것보다 개발하기전에 `개발환경` `git` 설정하는게 진짜 힘든 것 같다. Git은 미루다 미루다 이제 하는거지만 
Git command , eclipse에서 git 연동하는 것도 다시 한번 정리 해놔야 할것 같다. 


## VScode에 Git 연동하기 

### Git 설치하기
우선 Git을 연결하려면 `Git`을 깔아야한다. 여기서 말하는 Git은 `GitBash`를 말한다. [Git 설치하기](https://git-scm.com/download)  

### Git id,email설정 
```
$ git config --global user.name "13akstjq"
$ git config --global user.email "13akstjq@naver.com"
```
그 다음에 vscode를 재실행하면 vscode가 알아서 git을 잡아준다.  


## 원격저장소와 연결하기 
```
$ git push origin gh-pages
```
~/Desktop/vs-git
