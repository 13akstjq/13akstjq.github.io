---
layout: post
title:  "[React] 스크롤 부드럽게 움직이기"
date:   2019-06-17 11:24:00
author: 한만섭
categories: react
tags: react scroll smooth
---

* TOC
{:toc}


> ### react에서 scroll 움직이기 

[npm react-scroll바로가기](https://www.npmjs.com/package/react-scroll) 

```javascript
$ npm install react-scroll
```
위 명령어를 입력하면 설치가 됩니다. 설치가 되면 스크롤 이벤트를 해야할 컴포넌트가 있는 파일에서 아래 코드를 입력해줍니다.  

```javascript
import * as Scroll from 'react-scroll';
import { Link, Element , Events, animateScroll as scroll, scrollSpy, scroller } from 'react-scroll'
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

스크롤을 이동하기 

```javascript
scrollTo: function() {
    scroll.scrollTo(100);
  },
  scrollMore: function() {
    scroll.scrollMore(100);
  }
```
