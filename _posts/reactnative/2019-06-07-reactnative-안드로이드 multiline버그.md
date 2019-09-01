---
layout: post
title:  "[ReactNative] textinput 에서 multiline버그 "
date:   2019-06-07 22:00:00
author: 한만섭
categories: reactnative
tags: reactnative textinput multiline
---

* TOC
{:toc}


>## textline 버그 
자꾸 TextInput에서 returnkeytype을 done으로 해도 적용이 되지 않길래 봤더니 multiline 속성을 사용하고 있으면  
returnkeytype이 적용되지 않는 버그가 있었다. 안드로이드에서만 존재하는 것 같다.  
임시방편으로 일단 multiline은 사용하지 않은채 개발했다. 
