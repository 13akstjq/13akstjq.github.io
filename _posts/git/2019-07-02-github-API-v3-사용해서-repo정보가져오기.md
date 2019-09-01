---
layout: post
title:  "[GIT] github API v3 사용해서 repository 정보 가져오기"
date:   2019-07-02-01:02:00
author: 한만섭
categories: git
tags: git API
---

* TOC
{:toc}

> ### 정리할 내용 

  git 정보를 REST API를 통해서 가져오는 방법을 정리해보려고합니다. 사실 아래 참고 사이트만 읽어도 무방하지만 간단하게 정리해보자면 
  owner : user명 
  repo : repository
  
* user repo얻을 수 있는 call
  ```
  async function getRepos() {
        clear();
        const headers = {
          Accept: "application/vnd.github.nightshade-preview+json",
          Authorization: `Token b6b0e4efea3eb607fe0b23bcb5bc8f7d4d4269b1`
        };
        const url = "https://api.github.com/users/13akstjq/repos";
        const response = await fetch(url, {
          method: "GET",
          headers: headers
        });
        const result = await response.json();
        console.log(result);
      }
  ```
  
  
  
* event를 통해서 실시간 알림을 받을 수 있는데 해보기 
  
  
  
  
[참고사이트1](https://mingrammer.com/dev-commit-alarm-bot/)  
[참고사이트2](https://blog.outsider.ne.kr/1182)  
[github DOC](https://developer.github.com/v3/repos/#list-user-repositories)  
[유튜브](https://www.youtube.com/watch?v=5QlE6o-iYcE)
