---
layout: post
title:  "[인스타클론코딩] [FrontEnd].3 - Components 설계 "
date:   2019-07-04-:09:34:00
author: 한만섭
categories: clonecoding
tags: react instagram
---

* TOC
{:toc}

> ###  Auth 페이지 

로그인/회원가입 페이지는 기본적으로 아래와 같이 구성이 되어있습니다.  
![image](https://user-images.githubusercontent.com/46010705/60632190-3e7cb800-9e3f-11e9-9570-cd4cd27fb4ff.png)

- [X] Footer
- [] StateChanger
- [] Input Component
- [] Button Component
- [] Form 


* Footer Component

Footer는 **List(ul)**와 **Copyright(div)** 가 **Wrapper(div)**에 감싸져 있는 형태로 작성할 것입니다. List는 **ListItem(li)**
을 감싸고 있는 형태입니다.  


*  form 
form은 State의 상태에 따라서 아이디/비밀번호를 입력하거나, 회원가입 정보를 입력하는 form 을 보여주어야 
합니다. 그 안에서 Input Component와 Button Component를 사용하기 때문에 input과 Button Component를 제작해야 합니다. 


