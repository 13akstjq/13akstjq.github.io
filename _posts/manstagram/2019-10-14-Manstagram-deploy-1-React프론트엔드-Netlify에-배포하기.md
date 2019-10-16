---
layout: post
title: "Manstagram - deploy.1 - React프론트엔드 Netlify에 배포하기"
date: 2019-10-14-15:23:00
author: 한만섭
categories: manstagram
tags: React ReactHooks ReactNative Graphql Prisma Apollo
---



* TOC
{:toc}


## 1. front-end 배포하기 

front-end를 배포할 때 자주 사용하는 [netlify](https://www.netlify.com/)를 사용해보록 하겠습니다.   

netlify에서 모든 repository에 접근할 수 있도록 허용해줍니다.  

![1565614426553](../../../../assets/image/1565614426553.png)



### 1-1. front-end 프로젝트 선택 

![1565614493123](../../../../assets/image/1565614493123.png)



### 1-2. build 명령어 및 빌드 파일 경로 입력 

![1565614566154](../../../../assets/image/1565614566154.png)

첫번째 칸에는 

```bash
npm run build
```

두번째 칸에는 

```
build/
```

를 작성하고 **Deploy site**를 누르면 아래와 같이 배포를 진행한 후에 **site is live**라는 문구가 나오면 배포에 성공한 것입니다.  

![1565615354673](../../../../assets/image/1565615354673.png)





------

