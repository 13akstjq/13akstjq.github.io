---

layout: post
title:  "[debug] typescript에서 combineReducer사용하기"
date:   2019-09-24-13:06:00
author: 한만섭
categories: debug
tags: debug typescript redux combindreducer
---


* TOC
{:toc}


### 이슈 

typescript에서 combineReducer를 사용할 경우 아래와 같이 작성했는데 이슈가 발생했습니다.  



`reducer/index.tsx`

```js
import HeaderReducer from "../reducer/header/headerReducer";
import { combineReducers } from "redux";
import { ScrollDown } from "./header/headerTypes";


const rootReducer = combineReducers({
  HeaderReducer
});
export default rootReducer;

```



`component/Sidebar.tsx`

```js
 (...중략...)
 const Sidebar: React.FunctionComponent = () => {
 const isScrollDown = useSelector(
    state => state.HeaderReducer.isScrollDown
  );
  (...중략...)
  }
```

위와 같이 코드를 작성하면 state의 타입이 지정되지 않았다는 에러를 발생시킵니다.  타입스크립트를 사용하고 있기 때문에 state의 타입을 지정해줘야 하는 것 같아서 여러 시도를 해본 결과 아래와 같은 해결 방법을 알아냈습니다.  



### 해결 

루트리듀서의 state들은 결국 각 reducer들의 state들을 가지고 있는 것이기 때문에 아래와 같이 root리듀서의 인터페이스를 지정해줬습니다.  



`reducer/index.tsx`

```js
import HeaderReducer from "../reducer/header/headerReducer";
import { combineReducers } from "redux";
import { ScrollDown } from "./header/headerTypes";

export interface rootState {
  HeaderReducer: ScrollDown;
}

const rootReducer = combineReducers({
  HeaderReducer
});
export default rootReducer;

```

`headerTypes.tsx`

```js
/* ScrollDown */
export interface ScrollDown {
  isScrollDown: boolean;
}
```



위와 같이 rootReducer를 만드는 곳에 rootState라는 인터페이스를 만들었습니다. rootState는 HeaderReducer의 State를 담고 있습니다.    





```js
import HeaderReducer from "../reducer/header/headerReducer";
import { combineReducers } from "redux";
import { ScrollDown } from "./header/headerTypes";

export interface rootState {
  HeaderReducer: ScrollDown;
}

const rootReducer = combineReducers({
  HeaderReducer
});
export default rootReducer;

```



`component/Sidebar.tsx`

```js
 (...중략...)
 const Sidebar: React.FunctionComponent = () => {
 const isScrollDown = useSelector(
    (state: rootState) => state.HeaderReducer.isScrollDown
  );
  (...중략...)
  }
```

위와 같이 useSelector를 통해서 가져오는 state는 rootReducer의 state이기 때문에 컴바인된 모든 리듀서의 state타입을 담고 있는 rootstate 타입으로 정해줘야합니다.   



이렇게 작성하게 될 경우 combineReducer에 접근할 수 있습니다.  









