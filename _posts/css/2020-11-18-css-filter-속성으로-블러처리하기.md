---
layout: post
title: "[CSS] filter 속성으로 블러처리하기 "
date: 2020-11-18-13:43:59
author: 한만섭
categories: scss
tags: css blur html 블러 filter
---



* TOC
{:toc}




오늘은 css 속성인 filter를 이용해서 이미지를 블러처리하는 방법에 대해 정리해보겠습니다. 

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <meta http-equiv="X-UA-Compatible" content="ie=edge" />
        <title>Document</title>
        <link rel="stylesheet" href="./main.css" />
    </head>
    <body>
        <div class="container">
            <div class="box"></div>
            <div class="box"></div>
            <div class="box"></div>
        </div>
    </body>
</html>

```

```css
.container {
    display: flex;
}

.box {
    width: 100px;
    height: 100px;
    background: no-repeat url("./img_1.png") 0 / cover;
}

.box + .box {
    margin-left: 20px;
}

```

해당 경로에 이미지는 아무 이미지나 넣어주면 됩니다. 그런 상태로 실행시키면 아래처럼 노출되는데요. 

![image](https://user-images.githubusercontent.com/46010705/99498012-1c46f400-29ba-11eb-8d91-bf78ace77805.png)

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

### filter 속성 

블러처리는 시각적인 효과를 주기 위해서 많이 사용하고 있지만 브라우저 지원 범위가 좁아서 많이 사용하지는 못하는데요. 이 글에서는 filter 속성을 사용해서 블러처리를 구현해보려고 합니다. 

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <meta http-equiv="X-UA-Compatible" content="ie=edge" />
        <title>Document</title>
        <link rel="stylesheet" href="./main.css" />
    </head>
    <body>
        <div class="container">
            <div class="box blur"></div>
            <div class="box"></div>
            <div class="box"></div>
        </div>
    </body>
</html>

```

```css
.container {
    display: flex;
}

.box {
    width: 100px;
    height: 100px;
    background: no-repeat url("./img_1.png") 0 / cover;
}

.box + .box {
    margin-left: 20px;
}

.blur {
    filter: blur(5px);
    -webkit-filter: blur(5px);
}

```

위와 같이 .blur 클래스를 추가해준 후에 첫번째 box 요소에 blur 클래스를 추가해줍니다.

그러면 아래와 같이 첫번째 박스가 블러처리된 모습을 확인하실 수 있습니다. 

![image](https://user-images.githubusercontent.com/46010705/99498957-89a75480-29bb-11eb-9fe1-802781e618fb.png)

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

```css
filter: blur(5px);
-webkit-filter: blur(5px);
```

위 속성에서 `5px` 을 변화시켜주면 블러되는 정도를 조절할 수 있습니다. 

사파리 화면에서 확인해보면 아래 사진처럼 나오는 것을 확인하실 수 있습니다. 

![image](https://user-images.githubusercontent.com/46010705/99499091-b491a880-29bb-11eb-8526-c479401135ad.png)

아래 사진은 파이어폭스 브라우저에서 확인한 모습입니다. 

![image](https://user-images.githubusercontent.com/46010705/99499162-cffcb380-29bb-11eb-8aba-13d2db50cc80.png)

브라우저마다 약간의 차이는 있지만 블러처리가 되는 것을 확인하실 수 있습니다. 

다음에는 backdrop-filter 속성을 이용한 블러처리하는 방법을 정리해보도록 하겠습니다. 