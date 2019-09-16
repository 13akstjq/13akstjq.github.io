---
layout: post
title: "[typescript] typescript에서 match, location 사용하기"
date: 2019-09-16-17:21:00
author: 한만섭
categories: typescript
tags: react typescript location
---

- TOC
  
  {:toc}



## 정리할 내용 





기존의 리액트 프로젝트에서 현재 route에 대한 정보를 얻고 싶다면 **match,locaton,history**를 사용할 수 있었습니다. 하지만 typescript에서 사용할 경우 

![1568624133415](img/1568624133415.png)





```js
import React from "react";
import CarDetailPresenter from "./CarDetailPresenter";
import { RouteComponentProps } from "react-router-dom";

interface MatchParams {}

const CarDetailContainer: React.FunctionComponent<
  RouteComponentProps<MatchParams>
> = ({ match }) => {
  console.log(match);
  return <CarDetailPresenter></CarDetailPresenter>;
};

export default CarDetailContainer;

```

