---
layout: post
title:  "[responsive web] media query"
date:   2019-05-26 22:28:59
author: 한만섭
categories: html/css
tags:	html css responsiveweb
---

미디어쿼리는 스마트폰, 탭, 컴퓨터화면과 같이 여러 형태의 화면을 제공해야될 때 특성에 맞게 다른 스타일시트를 적용시킬 수 있게 합니다. 

미디어 쿼리는 기존의 css와 굉장히 충돌이 잘나는 것 같다. 

## 이슈

1.미디어뭐리를 염두한 프로젝트라면 style을 작성할 때 무조건 head에 style 태그를 만들어서 작성해야한다. 이렇게 하지 않으면 핸드폰화면으로 보더라도
태그가 호출될 때 style이 적용되기 때문에 head에서 미디어쿼리를 사용해도 적용이 되지 않는다. 

2.`bootstrap`을 사용한 프로젝트라면 `bootstrap`내에서 사용한 미디어쿼리로 인해서 페이지에서 적용이 되지 않는다. 
이 점을 해결하기 위해 페이지에서 style 을 주는 것이 아니라 `link`를 이용해 미디어쿼리를 적용시키면 에러발생을 줄일 수 있을 것 같다. 

3.css는 선택자를 구체적으로 설명하는 습관이 중요한것 같다.  태그와 클래스들을 명시해주는 것이 다른 태그와의 충돌이 발생하지 않게 해줄 것 같다. 



## 뷰포트(view port)

요즘과 같이 여러 해상도의 모바일기기가 존재할 때는 그에 맞는 반응형 웹을 구현해야 한다. 
반응형 웹을 구현하기 위해 저번에는 `미디어쿼리`를 사용했는데 이번에는 `뷰포트`를 사용해보려한다.


## 뷰포트란?

뷰포트는 화면이 실제로 보여지는 공간을 얘기한다. 웹 브라우저에서는 사용자가 켜놓은 웹 브라우저의 크기인데, 조절이 가능하다.
모바일의 경우에서는 사용자가 보고있는 디바이스 화면의 크기를 의미한다. 웹과 모바일이 뷰포트가 다르기 때문에 `뷰포트`가 필요한 것이다.
웹 프로젝트를 하면서  `<meta>` tag를 봤었는데 당장 중요한 것이 아니기 때문에 별 생각없이 넘어갔을 것이다. 

## 사용하기 

```javascript
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

위처럼 코드를 작성하게 되면 `content`에 속성값을 넣어주면 된다.
`width=device-width`일 경우 현재 사용자가 보고 있는 디바이스의 화면 크기를 의미한다.
`initial-scale=1.0`은 화면이 보여질때 zoom의 크기를 의미한다. 기본적으로 1.0으로 설정한다. 

# codecademy CSS VISUAL RULES

이번 강의에서는 css의 구조에 대해 배우는 것 같다. 

## css structure

```css
h1 {
  color: blue;
}
```

`color`는 `property`라고 부르며, `blue`는 `value`라고 부른다. 


```
font-weight : bold; // font-weight 는 글씨의 두께를 의미한다.
```

## Opacity

태그의 보여주는 정도를 조절하는 property
```
opacity: 0.5; // 0으로 가면 아예 안보임 1로가면 투명도 0  
```

## background-image

```css
.image{
  background-image: url("https://s3.amazonaws.com/codecademy-content/courses/freelance-1/unit-2/soccer.jpeg");
}
```


## Review

CSS declarations are structured into property and value pairs.  
The font-family property defines the typeface of an element.  
font-size controls the size of text displayed.  
font-weight defines how thin or thick text is displayed.  
The text-align property places text in the left, right, or center of its parent container.  
Text can have two different color attributes: color and background-color. color defines the color of the text, while background-color defines the color behind the text.  
CSS can make an element transparent with the opacity property.    
CSS can also set the background of an element to an image with the background-image property.  
