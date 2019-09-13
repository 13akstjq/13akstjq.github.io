---
layout: post
title: "[React] 리액트 프로젝트에서 innerHTML 사용하기 "
date: 2019-08-31-20:36:00
author: 한만섭
categories: react
tags: react innerHTML dangerouslySetInnerHTML
---

- TOC
  {:toc}

## 정리할 내용

바닐라 자바스크립트로 개발을 할 때는 _innerHTML_ 을 사용하게 되면 해당 태그 안에 _html_ 코드를 집어 넣을 수 있었습니다. 그렇다면 *js*로 만들어진 _react_ 프로젝트에서도 가능할 까요?

```js
import React from "react";

export default () => {
  const htmlCode = "<div><button>버튼 </button></div>";
  return <div>{htmlCode}</div>;
};
```

위 코드를 실행하면 아래와 같은 결과가 나옵니다.

![1567251856746](../../../../assets/image/1567251856746.png)

이유는 이게 가능하면 사용자가 임의로 스크립트코드를 집어넣을 수 있기 때문입니다. 보안 이슈를 막기위해 이렇게 작성하는 것입니다. 그렇다면 억지로라도 넣고싶다면 어떻게 해야할까요?

개발자가 "나는 개발자이고 이 문자열을 *html*형식으로 넣어줄게 내가 넣는거니까 안심하고 보여줘도돼~ " 라고 코드에게 얘기해준다면 원하는 결과를 얻을 수 있습니다. 이것을 하기 위해 ‘_dangerouslySetInnerHTML_’ 를 사용해야합니다.

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>

<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="9095928724"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

사용방법은 아래와 같이 작성하면 됩니다.

```js
import React, { useEffect } from "react";

export default () => {
  const htmlCode = "<div><button>버튼 </button></div>";
  return <div dangerouslySetInnerHTML={{ __html: htmlCode }}></div>;
};
```

결과는 아래와 같습니다.

![1567252091499](../../../../assets/image/1567252091499.png)

이렇게 html 코드를 사용하기 위해서는 위험하지만 알고 사용하는거라는 말을 코딩으로 해줘야합니다. 그 방법이 **_*dangerouslySetInnerHTML*={{ __html: htmlCode }}_** 이니까 필요한 경우에 위와 같이 코딩해주면 될 것 같습니다.

---

위 기능은 *fetch*를 해서 다른 사이트의 정보를 가져와서 내부에 넣어야 할 때 사용하면 유용하게 사용될 것 같습니다.
