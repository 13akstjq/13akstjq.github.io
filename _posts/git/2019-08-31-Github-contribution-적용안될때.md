---
layout: post
title:  "[Git] Github 잔디 안 심어질때 해결 방법"
date:   2019-08-31-00:20:00 
author: 한만섭
categories: git
tags: git github contribution github email
---

* TOC
{:toc}

##  















## Github 커밋했는데 잔디가 안심어질 때 





###  원인

github에 커밋을 했는데 잔디가 안심어지는 경우에는 github에 가입한 **email**과 gitbash에 등록한 **email**이 다를 경우에 위와 같은 문제가 발생합니다.  커밋이 안되는 프로젝트로 가서 아래 명령어를 쳐보면 git bash 에 등록된 email주소를 알 수 있습니다.  

```bash
git config --list
```

위 명령어를 쳤을 때 `user.email`에 적혀있는 email과 github email이 다르다면 문제가 발생합니다.  



### 해결방법 

gitbash를 켜서 아래 명령어를 쳐줍니다.  

```bash
git config --global user.email "여러분의 메일주소"
```



이렇게 git email설정을 global로 수정하게 되면 모든 곳에 적용이 되기 때문에 커밋을 해도 잔디가 생기지 않는 이슈를 해결할 수 있습니다.  



 

