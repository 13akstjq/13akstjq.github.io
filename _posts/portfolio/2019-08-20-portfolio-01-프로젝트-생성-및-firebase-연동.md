---
layout: post
title:  "[Portfolio][01] 프로젝트 생성 및 firebase 연동"
date:   2019-08-20-21:16:00
author: 한만섭
categories: portfolio
tags: portfolio
---





* TOC
{:toc}


![1571222266483](../../../../assets/image/1571222266483.png)

프로젝트로 제 포트폴리오를 만들어보려고 합니다. firebase로 backend를 구현할 예정이며 front는 create-react-app을 통해 제작하려고 합니다.  

***

## 1. 프로젝트 생성 

```bash
npx create-react-app mansub-portfolio
```

![1566235901690](../../../../assets/image/1566235901690.png)



### 1.1 새로만든 git repository에 remote 연결 



### 1.2 프로젝트 정리 후 hello world

![1566236642279](../../../../assets/image/1566236642279.png)

## 2. Firebase 연동하기 

프로젝트를 생성한 후에 제일 먼저 Firebase를 연동해보려고 합니다! 안해봤기 때문에 가장 프로젝트가 가벼운 상태에서 테스트를 해보는 편이 나을 것 같아서 먼저하기로 결정했습니다.  



firebase를 연동하기 전에 firebase가 어떤 것인지 간단하게 알아보도록 하겠습니다.  

### 2.1 firebase 소개 

1. RDBMS

```
* RDBMS는 사전에 데이터에 대해서 모두 설계가 되어야 한다. 
* 다른 데이터 베이스와 외래키를 통해서 관계를 정의
```

2. Document data model - NoSQL

```
* 고정되지 않은 데이터베이스 스키마
  * 처음 프로젝트를 시작할 때와 프로젝트가 커질 때 상당한 이점이 된다. 
  * 트리형태로 되어 있어서 데이터 간의 관계를 정의할 수 있다.  
  
* 쿼리 속도가 느림
  * SQL 쿼리를 사용할 수 없고 대신 트리 구조를 잘 만들어야 한다. 
  * 검색에 걸리는 노드의 개수가 많아지면 연산 속도가 매우 느려진다. 
  * 최근엔 Firestore의 등장으로 개선되었다고 한다. 
  
* JSON형태로 된 데이터 
  * Parsing할 필요가 없기 때문에 클라이언트와의 연동이 편하고 parse error의 가능성도 없다.
```

      <script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="4307878116"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

3. 클라우드 함수  

```
* Real-time datavase  
  * 데이터를 저장, 삭제, 변경  
  * Constistency 확보를 위한 후 처리와 분류  
* Firebase authentification  
  * 사용자 가입 및 탈퇴 등 사용자 정보 처리  
  
* Analytics  
  * Google analytics 이벤트 발생 시 처리   
  
* Cloud stoage  
  * storage (일종의 CDN) 관리  
  
* HTTP  
  * HTTP 요청에 대한 응답 생성  
  * RESTful API와 비슷   
  
* Cloud sub/pub  
  * Google cloud 시스템 간 메세징  
```

4. 그 외 편리한 기능들

```
* Authentication 모듈 제공  
  * 자체적으로 회원가입 및 로그인 기능과 UI를 제공   
  * SNS를 통한 SSO 연동도 가능 

* 클라우드 메세징   
  * 기기에 상관 없이 간편하게 푸시 알림 기능을 제공  
  
* Google analytics와의 연동  
  * 사용자가 서버로 보내는 요청을 기록해 보고서 작성  
  * 추적 코드에서 발생시키는 이벤트에 대한 핸들러를 구현할 수 잇음.  
```

​      

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="4307878116"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

### 2.2 firebase 연동하기 

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

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="4307878116"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

### 2.3  페이스북 로그인

firebase에 페이스북 로그인을 추가하기 위해서는 **Facebook**에서 앱을 만들어야합니다.

![1566299650037](https://user-images.githubusercontent.com/46010705/66911548-b2884800-f04b-11e9-97a0-62ed7de60182.png)

firebase Auth의 facebook에 url 정보가 나와있습니다.

![1566300009294](https://user-images.githubusercontent.com/46010705/66911600-cdf35300-f04b-11e9-9539-b284d72a4b6e.png)

그 url을 facebook App을 만들고 아래에 있는 곳에 입력해줍니다.

![1566299975663](https://user-images.githubusercontent.com/46010705/66911634-dfd4f600-f04b-11e9-98f9-6bbdbcef7253.png)

![1566310814710](https://user-images.githubusercontent.com/46010705/66911667-f4b18980-f04b-11e9-92c0-b6007d052fee.png)

### 2.4 github로그인

앱을 만들어서 clientID 와 KEY를 발급받습니다. 이것을 firebase Authentication에 입력해줍니다.

![1566311538622](https://user-images.githubusercontent.com/46010705/66911690-0430d280-f04c-11e9-8052-c1022c845198.png)

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="4307878116"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

### 2.4 firebase ui react

[공식 DOC](https://github.com/firebase/firebaseui-web-react)

#### - firebaseui 설치

```bash
npm install --save react-firebaseui
```

![1566311884557](https://user-images.githubusercontent.com/46010705/66911730-127eee80-f04c-11e9-851a-0b892b97333e.png)

firebaseui는 기본적인 ui를 제공하는 것과 styling할 수 있는 ui가 있습니다.

저는 firebaseui를 사용하지만 스타일은 제가 원하는 대로 변경하고 싶기 때문에 **styledFirebaseAuth**를 사용해보도록 하겠습니다.

![1566311830571](https://user-images.githubusercontent.com/46010705/66911763-20347400-f04c-11e9-9e4c-c451168ec98f.png)

```js
// Import FirebaseAuth and firebase.
import React from "react";
import StyledFirebaseAuth from "react-firebaseui/StyledFirebaseAuth";
import "../firebase/firebaseui-styling.global.css"; // Import globally.
import firebase from "firebase/app";
import "firebase/auth";

// Configure Firebase.
const config = {
	firbase 설정
};
firebase.initializeApp(config);

export default () => {
  const login = res => {
    localStorage.setItem("isLoggedIn", true);
    // console.log(res);
  };

  // Configure FirebaseUI.
  const uiConfig = {
    // Popup signin flow rather than redirect flow.
    signInFlow: "popup",
    // Redirect to /signedIn after sign in is successful. Alternatively you can provide a callbacks.signInSuccess function.
    callbacks: {
      // Avoid redirects after sign-in.
      signInSuccessWithAuthResult: res => login(res)
    },
    // We will display Google and Facebook as auth providers.
    signInOptions: [
      firebase.auth.GoogleAuthProvider.PROVIDER_ID,
      firebase.auth.FacebookAuthProvider.PROVIDER_ID,
      firebase.auth.GithubAuthProvider.PROVIDER_ID
    ]
  };

  return (
    <div>
      <StyledFirebaseAuth uiConfig={uiConfig} firebaseAuth={firebase.auth()} />
    </div>
  );
};

```

위 코드처럼 따로 `.css`파일을 만들어서 사용하면 firebaseUi의 style을 변경할 수 있습니다. 로그인을 하게 되면 로컬스토리지에 **isLoggedIn**을 true로 만들어줍니다.



