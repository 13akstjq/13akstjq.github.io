---
layout: post
title:  "[node] express서버와 mysql 시퀄라이징(sequelizing)으로 연동하기"
date:   2020-03-02 22:28:59
author: 한만섭
categories: nodejs
tags:	nodejs express mysql
---

* TOC
{:toc}
sequelize라는 ORM을 사용해서 mysql을 연동하는 방법입니다. 



### 프로젝트 생성 

```bash
express sequelize-study
cd sequelize-study
npm i 
```





### sequelize 설치 

```bash
npm i sequelize mysql2
```

`sequelize`명령어를 사용하기 위해 글로벌로도 설치해줍니다. 

 ```bash
sudo npm i -g sequelize-cli
 ```

초기화를 해줍니다. 

```bash
sequelize init
```

그러면 아래와 같이 폴더가 생성됩니다. 

![image](https://user-images.githubusercontent.com/46010705/76703877-5fa8de80-6718-11ea-86d3-35349b047793.png)

### 폴더 직접 작성해보기 

