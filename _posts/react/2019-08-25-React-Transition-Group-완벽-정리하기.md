---
layout: post
title:  "[React] react-transition-group으로 router간 animation구현하기"
date:   2019-08-25-20:03:00
author: 한만섭
categories: react
tags: react router react-transition-group
---



[TOC]





## React-Transition-Group 이란?

리액트의 컴포넌트에 **transition** 효과를 쉽게 줄 수 있는 공식 라이브러리 입니다. 이것을 이용해서 컴포넌트가 appear, enter, exit될 때 원하는 효과를 줄 수 있습니다.  



### 1. Transition 효과 사용 전 

기본적으로 router를 작성했을 경우 아래와 같이 url 주소가 변경되게 되면 그에 맞는 component를 보여주게 됩니다. 하지만 이런 Router전환은 딱딱하게 보입니다. 좀더 부드러운 Router전환을 해보고 싶을 때 **React-Transition-Group**을 사용하면 됩니다.  

 ![Honeycam 2019-08-25 20-27-06](../../../../assets/image/Honeycam 2019-08-25 20-27-06.gif)

`App.js`

```js
import React from "react";
import {
  NavLink,
  BrowserRouter as Router,
  Switch,
  Route
} from "react-router-dom";
import Home from "./Home";
import About from "./About";

import "./App.css";

const App = () => {
  return (
    <div className="App">
      <Router>
        <div className="nav">
          <NavLink exact to="/">
            Home
          </NavLink>
          <NavLink to="/about">about</NavLink>
        </div>
        <Switch>
          <Route exact path="/" component={Home}></Route>
          <Route path="/about" component={About}></Route>
        </Switch>
      </Router>
    </div>
  );
};

export default App;

```



***

 

### 2. Router에 transition 효과 적용시키기



우선 설치부터 해보도록 하겠습니다.  

```bash
npm install react-transition-group --save
```



설치를 완료한 후에 Router에 transition을 적용시키기 위해서는 두가지를 사용해야합니다.   





1. **CSSTransition**

   컴포넌트 하나에게 **Transition**효과를 줄 수 있는 컴포넌트입니다. 이 컴포넌트를 원하는 컴포넌트 상위에 사용하고 설정 값들을 넣어주면 효과를 적용할 수 있습니다.  

2. **TransitionGroup**

   Router 혹은 여러 컴포넌트에게 **CSSTransition**을 적용시키고 싶을 때 **CSSTransition**들의 상위에 사용하는 컴포넌트입니다. 이것을 사용할 경우 **map**함수와 같이  **CSSTransition**각각에게 **key**값을 부여해줘야 에러없이 사용할 수 있습니다.  



***



#### 2-1. CSSTransition

```js
import React from "react";
import {
  NavLink,
  BrowserRouter as Router,
  Switch,
  Route
} from "react-router-dom";
import Home from "./Home";
import About from "./About";

import "./App.css";

import { TransitionGroup, CSSTransition } from "react-transition-group";

const App = () => {
  return (
    <div className="App">
      <Router>
        <div className="nav">
          <NavLink exact to="/">
            Home
          </NavLink>
          <NavLink to="/about">about</NavLink>
        </div>
        <TransitionGroup>
          <CSSTransition timeout={300} classNames="fade">
            <Switch>
              <Route exact path="/" component={Home}></Route>
              <Route path="/about" component={About}></Route>
            </Switch>
          </CSSTransition>
        </TransitionGroup>
      </Router>
    </div>
  );
};

export default App;

```

위와 같이 라우터에 **CSSTransition**을 적용시킬 경우 **CSSTransition**에 **key**값을 부여해줘야합니다. 여기서 사용할 수 있는 방법이 아무것도 리턴하지 않는 **Route**를 하나 감싸주고 그 **Route**의 **key**값을 넘겨주는 것입니다.  왜냐하면 아래와 같이 **Route**는 key값을 가지고 있기 때문입니다. 

![1566733614621](../../../../assets/image/1566733614621.png)



그럼 **TransitionGroup**의 상위에 Route를 하나 추가하도록하겠습니다.  



```js


const App = () => {
  return (
    <div className="App">
      <Router>
        <div className="nav">
          <NavLink exact to="/">
            Home
          </NavLink>
          <NavLink to="/about">about</NavLink>
        </div>
        <Route
          render={({ location }) => {
            console.log(location);
            return (
              <TransitionGroup>
                <CSSTransition timeout={300} classNames="fade">
                  <Switch>
                    <Route exact path="/" component={Home}></Route>
                    <Route path="/about" component={About}></Route>
                  </Switch>
                </CSSTransition>
              </TransitionGroup>
            );
          }}
        ></Route>
      </Router>
    </div>
  );
};


```

Route는 props로 component혹은 render를 가질 수 있는 component에는 Home, About과 같은 import한 component를 넣어주면 되고 render는 태그를 직접 작성할 수 있습니다. 그래서 **render**를 해주고 transitionGroup을 return 해줬습니다. 이렇게 작성하면 콘솔에서 location을 확인할 수 있습니다. (모든 route는 location값을 가지고 있습니다. )  

![1566734026688](../../../../assets/image/1566734026688.png)

위에 나와있는 key는 겉에 감싸준 Route의 key값입니다. about일 경우와 home일 경우 다른 key값을 가지는 것을  확인할 수 있습니다. 그렇기 때문에 저 **key**값을 **CSSTransition**에 넘겨주면 될 것 같습니다.  

***



##### 이슈 1. react component로 router 작성하기 

여기서 시험삼아 컴포넌트를 아래와 같이  string만 return 해주는 형태로 만들었는데, 

```js
//About.js
export default () => "여기는 ABOUT 이구요";
```

이렇게 작성하면 아래와 같은 에러를 발생시킵니다.  

![1566734633932](../../../../assets/image/1566734633932.png)

그렇기 때문에 tag를 return 하는 component형태로 만들어 줍니다.  

```js
//About.js

import React from "react";

export default () => <div>"여기는 ABOUT 이구요."</div>;

```



***



위의 이슈를 해결하고 보면 아래와 같이 동작하는 것을 확인할 수 있습니다.   

***

 

![Honeycam 2019-08-25 21-05-56](../../../../assets/image/Honeycam 2019-08-25 21-05-56.gif)

***





뭔가 달라진 것 같지만 아직 제가 원하는 것처럼 동작하지 않습니다. 여기서 발생한 이슈를 정리해보겠습니다.  

1. 값은 무시하더라도, 각 라우터가 위치를 차지하고 있어서 자연스럽게 사라지고 나오는 것이 아니라  기존 라우터를 아래로 밀어내고 자리를 차지하고 있음.  
2. 하나의 컴포넌트는 **fade-out**이 되고, 하나의 컴포넌트는 **fade-in**되고 있으나, 두개의 컴포넌트가 같은 값을 보여주고 있음.  



***



#### 2-2. 라우터 position : absolute 적용시키기

첫번째 이슈를 해결하는 방법은 간단합니다. 각 route가 차지하는 공간을 absolute로 바꿔주게 되면 기존의 라우터 위로 fade-in 하고 기존의 라우터는 그 자리에서 fade-out하게 될 것입니다.  



아래와 같이 각 컴포넌트에게 **absolute**속성을 부여하도록 코드를 수정해보겠습니다.  

```js
import React from "react";

export default () => (
  <div style={{ position: "absolute", top: "0px", left: "0px" }}>
    "여기는 ABOUT 이구요."
  </div>
);

```



수정 후 동작화면  

***



![Honeycam 2019-08-25 21-12-28](../../../../assets/image/Honeycam 2019-08-25 21-12-28.gif)

***





수정을 하게 되면 위와 같이 서로 위치는 겹치게 동작하도록 만들 수 있습니다. 하지만 fade효과는 적용되지 않고 있습니다.  문제를 확인하기 위해 fade효과 css를 주석처리하고 timeout을 50000으로 늘려보겠습니다.  

```js
 <CSSTransition
                  key={location.key}
                  timeout={50000}
                  classNames="fade"
                >
```

![1566735442448](../../../../assets/image/1566735442448.png)

위와 같이 바꾸고 난 후에 react develop tool을 통해 확인해보도록 하겠습니다.  



***



#### 2-3. Switch에 location속성 부여하기 

- 사이트 처음 route 

![1566735593235](../../../../assets/image/1566735593235.png)



* about을 눌렀을 경우 

  ![1566735677480](../../../../assets/image/1566735677480.png)

위 처럼 모든 **CSSTransition**의 Route들이 about컴포넌트를 가르키고 있습니다. 그렇기 때문에 클릭을 계속 하게 될 경우 많은 Route들이 모두 Home혹은 About을 가리키게 되고 글씨의 두께가 두꺼워지는 것을 확인할 수 있습니다.    

![1566735767540](../../../../assets/image/1566735767540.png)

모든 Route가 같은 컴포넌트를 가르키는 것을 막기 위해서 **Switch**에게 **location**이라는 정보를 넘겨주어야합니다.  아래와 같이 코드를 수정해줍니다.  

```js
<TransitionGroup>
                <CSSTransition
                  key={location.key}
                  timeout={50000}
                  classNames="fade"
                >
                  <Switch location={location}>
                    <Route exact path="/" component={Home}></Route>
                    <Route path="/about" component={About}></Route>
                  </Switch>
                </CSSTransition>
              </TransitionGroup>
```

이렇게 작성하게 될 경우 현재 **Switch**의 하위에 있는 **Route**가 최 상위에 있는 **Route**가 넘겨준 **location**정보를 가질 수 있게 됩니다.  

최상위Route의 Location정보 -> Switch -> Route  

#### 중요 ) 그렇기 때문에 모든 Route가 현재의 location정보가 아닌 각자 자신이 받았던 location정보를 가질 수 있게 되는 것입니다.  

develop tool을 통해서 확인해보도록 하겠습니다.    

![Honeycam 2019-08-25 21-32-42](../../../../assets/image/Honeycam 2019-08-25 21-32-42.gif)

위 영상에서 글자들이 겹쳐보이는 것은 정상적인 결과입니다. 그 이유에 대해 말씀드리겠습니다.  

1. switch가 route에게 location정보를 넘겨주었기 때문에 각 route는 자기가 보여주어야할 컴포넌트를 보여주기 때문입니다.  
2. position : absolute이기 때문에 위치가 겹치기 때문입니다.  



지금 기존의 글자가 지워지지 않는 것은 fade 효과를 주석처리했기 때문인데 이것을 다시 300ms로 설정하게 될 경우 아래와 같이 정상적으로 동작하는 것을 확인할 수 있습니다.   

#### 2-4. 결과 

![Honeycam 2019-08-25 21-35-53](../../../../assets/image/Honeycam 2019-08-25 21-35-53.gif)



***



### 3. 정리하며 느낀 점 

이번에 정리를 할때 가장 도움을 많이 받은 것은 공식 DOC가 아닌 Youtube 영상에서 도움을 많이 받았는데 확실히 잘 만든 영상일 경우 문서보다 확실히 도움이 되는 것 같습니다. 기회가 된다면 직접 연습하는 영상을 녹화해서 보관하는 것도 좋을 것 같습니다.  

