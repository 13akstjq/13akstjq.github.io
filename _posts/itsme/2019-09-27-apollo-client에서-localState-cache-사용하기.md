---
layout: post
title: "[It's me] apollo-client에서 localState 및 cache사용하기 "
date: 2019-09-25-20:47:00
author: 한만섭
categories: itsme
tags: itsme apollo client localState cache
---



* TOC
{:toc}




### localState & cache

apollo-client에서 제공하는 localState를 프로젝트에 적용해보며 정리해보도록 하겠습니다.  



### 1. 초기화

`src/Apollo/LocalState.tsx`

```js
export const defaults = {
  isLoggedIn: Boolean(localStorage.getItem("token")) || false
};

export const resolvers = {
  Mutation: {
    logUserIn: (_: any, args: any, res: any): boolean => {
      const { cache } = res;
      const { token } = args;
      if (token !== "" && token !== undefined) {
        localStorage.setItem("token", token);
        cache.writeData({
          data: {
            isLoggedIn: true
          }
        });
        return true;
      } else {
        return false;
      }
    },
    logUserOut: (_: any, __: any, res: any): any => {
      const { cache } = res;
      localStorage.removeItem("token");
      cache.writeData({
        data: {
          isLoggedIn: false
        }
      });
      return null;
    }
  }
};

```