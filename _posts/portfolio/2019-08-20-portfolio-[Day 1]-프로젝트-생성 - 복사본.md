---
layout: post
title:  "[Portfolio][Day 1] 프로젝트 생성"
date:   2019-08-20-21:16:00
author: 한만섭
categories: portfolio
tags: portfolio
---











오늘부터 토이(?) 프로젝트로 제 포트폴리오를 만들어보려고 합니다. firebase로 backend를 구현할 예정이며 front는 create-react-app을 통해 제작하려고 합니다.  



## 프로젝트 생성 

```bash
npx create-react-app mansub-portfolio
```

![1566235901690](../../../../assets/image/1566235901690.png)



#### 새로만든 git repository에 remote 연결 



#### 프로젝트 정리 후 hello world

![1566236642279](../../../../assets/image/1566236642279.png)

### Firebase 연동하기 

프로젝트를 생성한 후에 제일 먼저 Firebase를 연동해보려고 합니다! 안해봤기 때문에 가장 프로젝트가 가벼운 상태에서 테스트를 해보는 편이 나을 것 같아서 먼저하기로 결정했습니다.  



```bash
npm install firebase
```

**firebase**설치는 오래걸리기 때문에 미리 해놓는 것을 추천드립니다.  

이제 react 프로젝트에 firebase 설정을 넣어야합니다.  

![1566237075808](../../../../assets/image/1566237075808.png)



`Firebase.js`

```js
import firebase from "firebase/app";
import "firebase/firestore";

const config = {
				비밀
};

firebase.initializeApp(config);

const firestore = new firebase.firestore();

export { firestore };

```

위와 같이 작성해주시면 됩니다. 그러면 프로젝트에서 아까 만든 firebase database를 사용할 준비가 된 것입니다. 그럼 이제 **App.js**에서 테스트를 해보도록 하겠습니다.  



`App.js`

```js
import React, { useEffect } from "react";
import { firestore } from "./Firebase";

function App() {
  useEffect(() => {
    firestore
      .collection("projects")
      .get()
      .then(() => console.log("성공"));
  }, []);

  return (
    <div className="App">
      <span>Hello World!</span>
    </div>
  );
}

export default App;

```

react hook인 **useEffect**를 사용했는데 이 것은 **ComponentDidMount**와 비슷한 역할을 해준다고 생각하시면 될 것 같습니다.  

promise방식은 **then**을 사용하는 것이 **async await**와 같은 역할을 해주는 것입니다.  

```js
firestore
      .collection("projects")
      .get()
      .then(() => console.log("성공"));
```

위처럼 작성하고 동작할 경우 다행히도 성공이라는 메세지를 console에서 확인할 수 있었습니다.  

![1566238088942](../../../../assets/image/1566238088942.png)

다음에는 collection을 확인하는 것은 해봤으니 이제 로그인을 할 수 있는 **Auth**를 건드려보도록 하겠습니다. 물론 오늘 말고 내일 정리하겠습니다~~