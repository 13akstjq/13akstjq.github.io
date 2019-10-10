---
layout: post
title: "[Portfolio][Day 2] Firebase 구글, 페이스북, 깃허브 로그인"
date: 2019-08-21-20:13:00
author: 한만섭
categories: portfolio
tags: portfolio
---

- TOC
  
  {:toc}

### 1. 페이스북 로그인

firebase에 페이스북 로그인을 추가하기 위해서는 **Facebook**에서 앱을 만들어야합니다.

![1566299650037](../../../../assets/image/1566299650037.png)

firebase Auth의 facebook에 url 정보가 나와있습니다.

![1566300009294](../../../../assets/image/1566300009294.png)

그 url을 facebook App을 만들고 아래에 있는 곳에 입력해줍니다.

![1566299975663](../../../../assets/image/1566299975663.png)

![1566310814710](../../../../assets/image/1566310814710.png)

### 2. github로그인

앱을 만들어서 clientID 와 KEY를 발급받습니다. 이것을 firebase Authentication에 입력해줍니다.

![1566311538622](../../../../assets/image/1566311538622.png)

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<!-- displayAd -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="2489269721"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

### 3. firebase ui react

[공식 DOC](https://github.com/firebase/firebaseui-web-react)

#### - firebaseui 설치

```bash
npm install --save react-firebaseui
```

![1566311884557](../../../../assets/image/1566311884557.png)

firebaseui는 기본적인 ui를 제공하는 것과 styling할 수 있는 ui가 있습니다.

저는 firebaseui를 사용하지만 스타일은 제가 원하는 대로 변경하고 싶기 때문에 **styledFirebaseAuth**를 사용해보도록 하겠습니다.

![1566311830571](../../../../assets/image/1566311830571.png)

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

