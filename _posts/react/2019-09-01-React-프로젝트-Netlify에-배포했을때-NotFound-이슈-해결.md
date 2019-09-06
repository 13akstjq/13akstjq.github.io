---
layout: post
title:  "[React] 리액트 프로젝트 Netlify에 배포했을 때 새로고침시 NotFound "
date:   2019-09-01-11:58:00
author: 한만섭
categories: react
tags: react Netlify 

---



* TOC
{:toc}


## 정리할 내용 

제가 리액트로 만든 프로젝트를 Netlify로 배포했을 때 _Router_이동은 잘 되는데 새로고침시 아래와 같은 에러가 발생했는데, 그 이슈를 해결하는 방법을 찾아서 정리해 보려고 합니다.  ![1567306823920](../../../../assets/image/1567306823920.png)





### 원인 

위와 같은 이슈가 발생하는 원인을 찾다가 [이 사이트](https://www.slightedgecoder.com/2018/12/18/page-not-found-on-netlify-with-react-router/)에서 해답을 얻을 수 있었습니다. 원인은 _Netlify_에 _Redirect_설정을 하지 않았기 때문에 처음에 사이트를 어디로 _Redirect_시킬 것인지 모르기 때문입니다.  

![Honeycam 2019-09-01 12-02-10](../../../../assets/image/Honeycam 2019-09-01 12-02-10.gif)

위 처럼 링크에 직접 경로를 칠때도 동일한 이슈를 띄우는데 이것도 _Redirect_설정을 해주면 됩니다.  



### 해결책 

역시 모든 해결은 공식 문서에 나와 있듯이 이번에도 [Netlify](https://www.netlify.com/docs/redirects/)에 해결방법이 나와있었습니다.  

![1567307193862](../../../../assets/image/1567307193862.png)

![1567307098220](../../../../assets/image/1567307098220.png)

모든 경로 안에 있는 index.html 로 200응답이 왔을 때 가라! 라고 설정을 해줬습니다.  



***

