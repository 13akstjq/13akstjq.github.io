---
layout: post
title:  "[인스타클론코딩] [Server].20 - counting하기   "
date:   2019-06-29-02:23:00
author: 한만섭
categories: clonecoding
tags: instagram post fragment
---

> ### 정리할 내용 
 
좋아요 갯수와 같이 Model의 갯수를 세야하는 상황이 있습니다. 그런 경우에 어떻게 하는지 알아보도록 하려고합니다.  


> ### 코드 

  ```
   likeCount: async (parent, _, __) =>
            prisma
                .likesConnection({ where: { post: { id: parent.id } } }) // post중에 부모의 id 와 같은 post의 like들을 연결시킴 
                .aggregate()  // 
                .count()  // 갯수 세기 
  ```
