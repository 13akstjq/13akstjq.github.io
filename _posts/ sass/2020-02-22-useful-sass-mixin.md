---
layout: post
title:  "[sass] 유용한 sass mixing"
date:   2020-02-22 22:28:59
author: 한만섭
categories: sass
tags:	sass mixing
---

* TOC
{:toc}
자주 사용하는 `mixin`을 정리하는 글입니다.

-  `clearfix`

  `float` 된 요소를  `clear`하는 `mixin`

  ```scss
  @mixin clearfix {
    &::after {
      content: "";
      display: block;
      clear: both;
    }
  }
  ```

- `text-ellipsis`

  인자로 받은`line` 만큼 말줄임 해주는 `mixin` 인자를 넘기지 않으면 1줄 말줄임

  ```scss
  @mixin ellipsis($line: 1) {
    @if ($line == 1) {
      overflow: hidden;
      text-overflow: ellipsis;
      white-space: nowrap;
    } @else {
      display: -webkit-box;
      overflow: hidden;
      text-overflow: ellipsis;
      -webkit-box-orient: vertical;
      -webkit-line-clamp: $line;
    }
  }
  ```

- `background` -  **리팩토링 필요**

  스프라이트 이미지 처리를 위한 `mixin`

  ```scss
  @mixin background($url, $x, $y, $width, $height) {
    background-image: url($url);
    background-position: ($x) ($y);
    background-size: $width $height;
  }
  ```

- `media-query`

  `map`구조인 `devices`변수를 순회하여 인자로 받은 `device`에 맞는 값으로 `media-query`문을 작성해주는 `mixin`

  ```scss
  $devices: (
    mobile: 320px,
    tablet: 768px,
    desktop: 1280px
  );
  
  @mixin mq($device) {
    @if map-has-key($devices, $device) {
      $device-width: map-get($devices, $device);
      @media screen and (min-width: #{$device-width}) {
        @content;
      }
    } @else {
      @warn 'Invalid breakpoint: #{$device}.';
    }
  }
  ```

- `inline-block`

  `inline-block` 에 `vertical-align : top`을 일일히 붙히지 않아도 되는 `mixin`

  ```scss
  @mixin inline-block-box($vertical-align: top) {
    display: inline-block;
    vertical-align: $vertical-align;
  }
  ```

  





## 종합

`_variables.scss`

```
$devices: (
  mobile: 320px,
  tablet: 768px,
  desktop: 1280px
);

```



`_mixins.scss`

```scss
@import "variables";

// float clear
@mixin clear {
  &::after {
    content: "";
    display: block;
    clear: both;
  }
}

@mixin ellipsis($line: 1) {
  @if ($line == 1) {
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  } @else {
    display: -webkit-box;
    overflow: hidden;
    text-overflow: ellipsis;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: $line;
  }
}

@mixin background($url, $x, $y, $width, $height) {
  background-image: url($url);
  background-position: ($x) ($y);
  background-size: $width $height;
}

@mixin mq($device) {
  @if map-has-key($devices, $device) {
    $device-width: map-get($devices, $device);
    @media screen and (min-width: #{$device-width}) {
      @content;
    }
  } @else {
    @warn 'Invalid breakpoint: #{$device}.';
  }
}

@mixin inline-block-box($vertical-align: top) {
  display: inline-block;
  vertical-align: $vertical-align;
}

```



