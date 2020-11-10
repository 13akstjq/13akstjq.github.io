---
layout: post
title:  "[Firebase] Firebase database 규칙 수정하기"
date:   2019-06-29-10:34:00
author: 한만섭
categories: firebase
tags: google firebase
---

* TOC
{:toc}


## Firebase Database 규칙이란 

firebase를 이용한 데이터 베이스를 구현하는 경우 파이어 베이스의 데이터 베이스에 접근할 수 있는 접근 권한을 부여해야합니다. 

firebase의 Database메뉴의 규칙 탭에 들어가면 처음에는 아래와 같은 코드가 작성이 되어있습니다. 

```js
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {

    // This rule allows anyone on the internet to view, edit, and delete
    // all data in your Firestore database. It is useful for getting
    // started, but it is configured to expire after 30 days because it
    // leaves your app open to attackers. At that time, all client
    // requests to your Firestore database will be denied.
    //
    // Make sure to write security rules for your app before that time, or else
    // your app will lose access to your Firestore database
    match /{document=**} {
      allow write, read;
    }
  }
}
```

이 상태로 계속 파이어베이스를 사용하다보면 아래와 같이 메일이 옵니다. 아래 메일은 Firebase의 데이터 베이스에 아무나 접근할 수 있으니 규칙의 권한을 부여하라는 메일입니다.

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<!-- n잡 블로그 사각형 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="2552901794"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

![image](https://user-images.githubusercontent.com/46010705/86005474-aadcc800-ba4f-11ea-87ce-d41f621038c2.png)

## firebase database 규칙 수정

```js
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {

    // This rule allows anyone on the internet to view, edit, and delete
    // all data in your Firestore database. It is useful for getting
    // started, but it is configured to expire after 30 days because it
    // leaves your app open to attackers. At that time, all client
    // requests to your Firestore database will be denied.
    //
    // Make sure to write security rules for your app before that time, or else
    // your app will lose access to your Firestore database
    match /{document=**} {
      allow write: if request.auth.uid != null ;
      allow read: if request.auth.uid != null;
    }
  }
}
```

위와 같이 firebase database 규칙을 수정할 수 있습니다. 

제가 작성한 규칙은 Write,Read를 할 때 request 즉, 요청 값에 auth.uid가 존재하는 경우에만 해당 동작을 할 수 있게 허용한 것입니다. 

이렇게 할 경우 firebase 의 auth를 통해 로그인하지 않은 유저들은 저의 firebase database 에 접근을 할 수 없게 됩니다. 

위의 방법 외에도 database의 collection별로 다른 규칙도 부여할 수 있습니다. 