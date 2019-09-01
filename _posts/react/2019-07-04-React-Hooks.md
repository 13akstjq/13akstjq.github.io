---
layout: post
title:  "[React] - React Hooks  "
date:   2019-07-04-:16:00:00
author: 한만섭
categories: react
tags: react hooks
---

* TOC
{:toc}

리액트에서 class 를 function으로 사용할 수 있게 해줌. 
함수형 프로그래밍을 할 수 있게 해주는 것. 
코드를 더 좋게 만들어 줄 수 있음.  

recompose 팀이 만들던 것을 React에서 인수해서 릴리즈 해서 사용한 것

훅을 만들면 javascript의 document.getElementById와 같은 것을 사용할 수 있음.  

기존의 handling input, Fetching이 불편했기 때문에 나온 것이 hooks입니다.  

반복되는 작업이 불편함이 일 때 데이터를 가져오고 인풋을 업데이트를 할때 사용할 수 있음.  

어느 API에서든 편하게 데이터를 요청할 수 있는 것.  

(...name) ===  value=(name.value)

use Fetch는 url을 가져오기 위해서 사용하는 것. 첫번째 payload를 가져옴.  

axios는 http라이브러리 인데 fetch보다 괜춘함. 

useEffect는 componenetDidMount()를 위해서 있당.  

> ### 버튼으로 숫자 증가 감소 

```
import React, { useState } from "react";
import ReactDOM from "react-dom";

import "./styles.css";
const numb = (ini) => {
  const [num, setNum] = useState(ini);
  return {
    num,
    setNum
  }
} 

function App() {
  const {num,setNum} = numb(1);
  return (
    <div className="App">
      <h1>Hello </h1>
      {num}
      <button onClick={() => setNum(num+1)}>증가</button>
      <button onClick={() => setNum(num-1)}>감소</button>
    </div>
  );
}

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);

```


> ### useState를 사용해서 Input Value값 셋팅하기 , 변경 감지, 유효성 체크 

{...name} 은 spreadoperator를 사용해서 name을 펼쳐서 보여주게 됩니다. 혹은 `value={name.value}`라고 적어도 됩니다.  


```
import React, { useState } from "react";
import ReactDOM from "react-dom";

import "./styles.css";

// valitating function 이 true면 return 하기
// validating fuctnion 만들기

const useInput = (initial, validateFuc) => {
  const [value, setValue] = useState(initial);
  const onChange = event => {
    const {
      target: { value }
    } = event;
    let willUpdate = true;
    if (typeof validateFuc === "function") {
      willUpdate = validateFuc(value.length);
    }
    if (willUpdate) {
    setValue(value);
    }
  };

  return { value, onChange };
};

function App() {
  const maxLen = value => value <= 10;
  const name = useInput("Mr. ", maxLen);
  return (
    <div className="App">
      <h1>Hello </h1>
      <input {...name} />
    </div>
  );
}

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```


> ### useTab

```
import React, { useState } from "react";
import ReactDOM from "react-dom";

import "./styles.css";

const posts = [
  { title: "first", content: "first_content" },
  { title: "second", content: "second_content" }
];

const useTab = (initialIndex, allPosts) => {
  const [currentIndex, setCurrentIndex] = useState(initialIndex);

  return {
    currentItem: allPosts[currentIndex],
    onChange: setCurrentIndex
  };
};

function App() {
  const { currentItem, onChange } = useTab(0, posts);
  console.log(currentItem);
  return (
    <div className="App">
      <h1>Hello </h1>
      {posts.map((post, index) => (
        <button onClick={()=>onChange(index)}>{post.title}</button>
      ))}
      {currentItem.content}
    </div>
  );
}

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```




