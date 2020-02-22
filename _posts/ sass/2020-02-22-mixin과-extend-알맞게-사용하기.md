---
layout: post
title:  "[sass] mixin과 extend 알맞게 사용하기"
date:   2020-02-22 22:28:59
author: 한만섭
categories: sass
tags:	sass extend mixin
---

* TOC
{:toc}


## 정리할 내용

`sass`문법에서 가장 많이 사용하는`mixin`,`extend,%placeholder` 에 대해서 어떤 상황에 어떤 문법을 쓰는 것이 적절한지 생각을 정리하는 글입니다. 

***

결론부터 말하자면, `mixin`은 **소스코드의 중복을 막기 위해**사용하고, `extend,%placeholder`은 **연관성있는 규칙에 만들기 위해**사용합니다. 

그렇다면 **소스코드의 중복**과 **연관성있는 규칙**이 무엇인지 알아보겠습니다. 

### mixin으로 연관성없는 선택자의 중복 코드 제거

아래 코드처럼 `.scss`를 작성했다고 생각해보겠습니다. 

```scss
%site_color {
  color: #f00;
}

/* ...중략 */

.main_conatiner {
  @extend %site_color;
}

/* ...중략 */

.view_container {
  @extend %site_color;
}

/* ...중략 */

.detail_container {
  @extend %site_color;
}

```

위에 작성한 `.main_conatiner`,`.view_container`,`.detail_container`는 서로 연관성이 없는 클래스입니다. 하지만 해당 클래스의 `color`값이 동일하다는 이유로 `site_color`라는 값을 모두 `extend`해서 사용하고있습니다. 위 코드를 컴파일하면 아래 `.css`파일이 생성됩니다. 

```scss
.main_conatiner, .view_container, .detail_container {
  color: #f00; 
}

/* ...중략 */

.main_conatiner {}

/* ...중략 */

.view_container{}

/* ...중략 */

.detail_container {}
```

위 코드를 보면, 전혀 연관성이 없는클래스들이 같이 선언이 되어있기 때문에 수백줄이 떨어져있던 클래스가 한 곳에 작성이 되는 불규칙한 상황이 발생했습니다. 아마 `pure CSS`로 작성을 했다면 아래처럼 작성을 했을 것 같습니다.

```scss
.main_conatiner {
  color: #f00;
}

/* ...중략 */

.view_container{
  color: #f00;
}

/* ...중략 */

.detail_container {
  color: #f00;
}
```

서로 연관성이 없는 클래스에 단순히 같은 속성을 부여하기 위해서 `extend`를 사용할 경우 이러한 문제점을 만드는 것을 확인했습니다. 위 코드처럼 컴파일을 하기위해서 사용하는 `sass`문법이 `mixin`입니다. **연관성이 없는 반복되는 규칙을 따로 만들어서 사용**하는 것입니다. 

```scss
@mixin site_color {
  color: #f00;
}

.main_conatiner {
  @include site_color;
}

.view_container {
  @include site_color;
}

.detail_container {
  @include site_color;
}

//CSS
.main_conatiner {
  color: #f00; }

.view_container {
  color: #f00; }

.detail_container {
  color: #f00; }

```

**연관성이 없는 선택자에 mixin을 사용했을 경우**에 위 처럼  컴파일 후에도 연관성 없는 선택자들이 따로 선언되어있는 것을 확인할 수 있습니다. 그렇다면 도대체 `extend,%placeholder`는 언제 사용하는거지? 라는 의문을 가질 수 있습니다. 

### extend를 이용해 선택자간의 연관성 형성하기 

위에서는 서로 연관 없는 선택자를 사용했지만 이번에는 같은 버튼이지만 종류가 다른 버튼에 대한 클래스로 예를 들어보겠습니다. 

```scss
.btn {
  width: 100px;
  height: 80px;
}

.btn_success {
  @extend .btn;
  color: green;
}

.btn_danger {
  @extend .btn;
  color: red;
}

.btn_warning {
  @extend .btn;
  color: orange;
}

// CSS
.btn, .btn_success, .btn_danger, .btn_warning {
  width: 100px;
  height: 80px; }

.btn_success {
  color: green; }

.btn_danger {
  color: red; }

.btn_warning {
  color: orange; }

```

위와 같이 작성할 경우 연관성있는 선택자를 묶을 수 있지만 불필요한 `.btn`선택자가 생기는 것을 막고 싶다면 `%placeholder`를 사용하면 됩니다. 

```scss
%btn {
  width: 100px;
  height: 80px;
}

.btn_success {
  @extend %btn;
  color: green;
}

.btn_danger {
  @extend %btn;
  color: red;
}

.btn_warning {
  @extend %btn;
  color: orange;
}

// CSS

.btn_success, .btn_danger, .btn_warning {
  width: 100px;
  height: 80px; }

.btn_success {
  color: green; }

.btn_danger {
  color: red; }

.btn_warning {
  color: orange; }

```

위처럼 작성할 경우 버튼에 대한 **연관된 속성**은 한 곳에서 선언하고, 서로 다른 속성은 해당 선택자 규칙에서 선언이 되었습니다. 



### 결론 

**선택자간의 연관성이 존재한다면** `extend`를 사용하고, **연관성은 없지만 코드가 겹치는 선택자들**이라면 `mixin`으로 소스코드의 중복을 없애기 위해 사용해야합니다. 

[참고 사이트](https://mytory.net/2016/12/23/when-to-use-extend-when-to-use-a-mixin.html)



