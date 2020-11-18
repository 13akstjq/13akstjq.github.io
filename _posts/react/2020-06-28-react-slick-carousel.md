---
layout: post
title:  "react-slick으로 캐러셀 구현하기 "
date:   2020-06-28 21:27:59
author: 한만섭
categories: react
tags: React carousel react-slick
---

* TOC
{:toc}


## react-slick

React-slick은 리액트 프로젝트에서 캐러셀을 구현하기 쉽게 도와주는 모듈입니다. 



### 설치

**npm**

```bash
npm install react-slick --save
```

**yarn**

```bash
yarn add react-slick
```

이외에도 기본적인 slick-carousel 모듈도 설치를 해주어야합니다. 

```
npm install slick-carousel
 
// Import css files
import "slick-carousel/slick/slick.css";
import "slick-carousel/slick/slick-theme.css";
```

혹은 cdn을 이용해 아래처럼 추가를 해줄 수 도 있습니다. 

```html
<link rel="stylesheet" type="text/css" charset="UTF-8" href="https://cdnjs.cloudflare.com/ajax/libs/slick-carousel/1.6.0/slick.min.css" />
<link rel="stylesheet" type="text/css" href="https://cdnjs.cloudflare.com/ajax/libs/slick-carousel/1.6.0/slick-theme.min.css" />
```

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

### 사용하기 

```jsx
import React from "react";
import Slider from "react-slick";
 
class SimpleSlider extends React.Component {
  render() {
    var settings = {
      dots: true,
      infinite: true,
      speed: 500,
      slidesToShow: 1,
      slidesToScroll: 1
    };
    return (
      <Slider {...settings}>
        <div>
          <h3>1</h3>
        </div>
        <div>
          <h3>2</h3>
        </div>
        <div>
          <h3>3</h3>
        </div>
        <div>
          <h3>4</h3>
        </div>
        <div>
          <h3>5</h3>
        </div>
        <div>
          <h3>6</h3>
        </div>
      </Slider>
    );
  }
}
```

`react-slick` 의 Slick을 import해서 사용할 수 있으며 기본적인 설정 값만 넣어주면 바로 사용할 수 있습니다. 

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

```json
var settings = {
      dots: true, // 캐러셀의 점을 보여줄 것인지
      infinite: true, // 마지막 장 다음에 첫번째가 나오게 할 것인지
      speed: 500, // 넘어가는 속도는 몇으로 할 것인지
      slidesToShow: 1, 
      slidesToScroll: 1
    };
```

위와 같이 사용하면 아래 영상처럼 캐러셀을 사용할 수 있습니다.

![Jun-29-2020 21-50-19](https://user-images.githubusercontent.com/46010705/86007566-8d5d2d80-ba52-11ea-87b9-02c39448e171.gif)

