---

layout: post
title:  "[debug] redux state가 undefine 에러 "
date:   2019-09-24-13:06:00
author: 한만섭
categories: debug
tags: debug typescript redux state
---


* TOC
{:toc}




### 이슈 

이 이슈는 사실 다른 이유에서도 발생할 수 있는 것이지만 리듀서를 만들 때 범하기 가장 쉬운 실수라 생각 됩니다.  



`reducer/headerReducer.tsx`

```json
import { SCROLL_DOWN, HeaderActionType, ScrollDown } from "./headerTypes";

const initialState: ScrollDown = {
  isScrollDown: false
};

const headerReducer = (
  state = initialState,
  action: HeaderActionType
): ScrollDown => {
  switch (action.type) {
    case SCROLL_DOWN: {
      return {
        ...state,
        isScrollDown: action.isScrollDown
      };
    }
    default:
      return state;
  }
};

export default headerReducer;

```

보통 리듀서를 작성할 때 위와 같이 작성하는데 여기서 **default**에 대해서 크게 생각을 하고 있지 않다가 직접 리듀서를 작성할 때 아래와 같이 default를 빼먹고 작성해버렸습니다.  이렇게 작성할 경우 



```js
import { SCROLL_DOWN, HeaderActionType, ScrollDown } from "./headerTypes";

const initialState: ScrollDown = {
  isScrollDown: false
};

const headerReducer = (
  state = initialState,
  action: HeaderActionType
): ScrollDown => {
  switch (action.type) {
    case SCROLL_DOWN: {
      return {
        ...state,
        isScrollDown: action.isScrollDown
      };
    }
  }
};

export default headerReducer;

```



- 에러가 발생한 이유 

이유에 대해선 제 생각이기 때문에 틀릴 수 도 있습니다.  

createStore를 통해서 state이렇게 작성할 경우 초기에 state를 initialState로 초기화 해야하는데 store를 만들 때 내부적으로 switch case의 default를 통해서 state를 return 하는 것 같습니다. 근데 default를 정하지 않을 경우 초기 state를 return 하지 못해서 undefine 에러가 뜨는 것 같습니다.  



### 결론 



#### switch case의 default 를 까먹지 말자 !!!









