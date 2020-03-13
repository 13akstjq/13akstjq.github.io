---
layout: post
title:  "[CSS] Flex Box 속성 정리"
date:   2020-03-10 15:30:00
author: 한만섭
categories: css
tags: css Flex 
---

* TOC
{:toc}
### Flexbox란?

기존에 컨텐츠들을 수평으로 배치할 때  `float` 혹은 `inline-block` 로 마크업할 때의 불편함을 해결하여 쉽게 배치할 수 있도록 CSS3에 추가된 방식입니다. 

크게 컨텐츠들을 감싸는  `Flex Container` 와 각 컨텐츠들인 `Flex Item`으로 구성되어있어 각 요소에 적절한 속성과 값을 적용해서 원하는 레이아웃을 구성할 수 있습니다. 

### Flexbox 지원 범위 

Flexbox는 좋은 기능인 만큼 이전 버전의 브라우저에서 지원을 안해주기도 합니다. 실제 서비스를 개발하기 위해선 이를 잘 알고 써야합니다. 

![image](https://user-images.githubusercontent.com/46010705/76543864-72c66f00-64ca-11ea-818a-c474374a5613.png)

IE는 9버전 이하는 지원을 하지 않으며, 10,11에서도 버그가 존재합니다. 모바일 웹에서는 지원을 잘해주고 있습니다. 서비스가 어떤 브라우저까지 지원하는지에 따라 적절하게 사용하면 될 것 같습니다. 

### Flex Container 주요 속성

| 속성            | 용도                                 |
| --------------- | ------------------------------------ |
| display         | Flex Container로 만들기 위함.        |
| flex-direction  | flex item의 주축 방향 선언           |
| flex-wrap       | flex item의 줄바꿈 선언              |
| flex-flow       | flex-direction과 flex-wrap 축약 속성 |
| justify-content | 주축 정렬 방법                       |
| Align-content   | 교차축 줄(line) 정렬 방법            |
| align-items     | 교차축 컨텐츠 정렬 방법              |



#### display

| 값          | 의미                                 |
| ----------- | ------------------------------------ |
| flex        | **Block** 엘리먼트인 Flex Container  |
| inline-flex | **inline** 엘리먼트인 Flex Container |

`display`속성을 통해 엘리먼트를 `Flex Container`로 만들 수 있는데, `Flex Container`를 `Block`레벨로 만들 것인지 `inline`레벨로 만들 것인지에 따라 속성 값을 정할 수 있습니다. 



#### flex-flow

```css
.flex-container { 
	display : flex; 
  flex-direction : column;
  flex-wrap : wrap;
}

/* flex-flow로 축약 */
.flex-container { 
	display : flex;
  flex-flow : column wrap;
}
```

`flex-direction`과 `flex-wrap`은 함께 자주 사용하는 속성이기 때문에  `flex-flow`속성 하나로 축약해서 사용할 수 있습니다. 



#### justify-content

`flex-item`의 주축 정렬 방법을 정할 수 있습니다.  `Flex Container`의 기본 주축 방향은 `row` 입니다. flex item은 왼쪽부터 오른쪽으로 채워집니다. 

`justify-content`는 `flex-item`들이 주축인 수평방향에서 좌측 정렬,우측 정렬, 가운데 정렬 등 여러 정렬 방법에 대해 정할 수 있는 속성입니다.

 여기서 주의할 점은 **주축**이 항상 수평방향이 아니라 `flex-direction`의 값에 따라 변한다는 것입니다.

| flex-direction 값 | 주축 방향 |
| ----------------- | --------- |
| **row** (default) | 행 (수평) |
| **column**        | 열 (수직) |



#### align-content 와 align-item 차이점 

**align-item** : `justify-content`와 반대로 교차 축의 정렬 방법 설정

**align-content** : `Flex Container`의  `flex Item`들이 두줄 이상으로 배치되어있을 경우에 각 줄을 어떻게 배치할 것인지 설정. **`flex item`이 한줄로 배치되어 있을 경우 동작 안함.**



### Flex item 주요 속성

| 속성        | 용도                          |
| ----------- | ----------------------------- |
| order       | Flex Item 순서 설정           |
| flex-grow   | Flex Item 너비 증가 비율 설정 |
| flex-shrink | Flex Item 너비 감소 비율 설정 |
| flex-basis  | Flex Item 기본 너비 설정      |
| align-self  | 교차 축의 item 정렬 방법 설정 |



#### order

`Flex Item`의 순서를 정할 수 있는 속성으로 기본 값은 0이고 숫자가 클수록 나중에 배치됩니다. 



#### flex-grow

`Flex Item`의 너비 증가 비율을 정하는 속성입니다. 

 기본 값은 0으로 `Flex Container`의 너비가  `Flex Item`의 너비보다 커도 `Flex Item`은 커지지 않습니다.

 0보다 큰 값이 들어올 경우 다른 `Flex Item`과 비율을 나눠서 증가합니다. 

```css
.flex_container { 
	display : flex;
}

.flex_item1 { 
	flex-grow : 1; 
}

.flex_item2 { 
  flex-grow : 2; 
}
```

위와 같을 경우 `flex_container`의 크기를 키우면   `flex_item2`과 `flex_item1`은 2:1비율로 너비가 증가하게 됩니다. 

주의해야할 점은 다른 `flex_item`에 따라 **상대적**이라는 것 입니다. 

#### flex-shrink

`Flex Item`의 너비 감소 비율을 정하는 속성입니다. 

 기본 값은 1으로 `Flex Container`의 너비가  `Flex Item`의 너비보다 작아지면 `Flex Item`의 너비는 그에 맞게 작아집니다. 

 0일 경우 `Flex Container`의 크기가 자신보다 작아져도 너비는 줄어들지 않습니다. 

`flex-grow`속성과 마찬가지로 다른 `flex item`들의 `flex-shrink` 값에 따라 감소 비율이 정해지게 됩니다. 

#### flex-basis

`Flex Item`의 기본 너비를 설정하는 속성입니다. 이 속성을 정의하면 `flex-shrink`속성도 기본 값으로 정의 됩니다. `flex Container`의 크기가 `flex Item`들의 크기보다 작아졌을 경우 서로 같은 비율로 작아지게 하기 위해서 입니다. 



#### flex

`flex-grow`,`flex-shrink`,`flex-basis`를 축약한 속성입니다. 

```css
.flex_item { 
  flex : {flex-grow} {flex-shrink} {flex-basis};
  flex : 0 1 auto;/* default */
}
```

`flex-grow`를 제외한 속성은 생략가능합니다. 

```css
.flex_item { 
	flex : 1; 
}
/* 아래와 동일 */
.flex_item { 
	flex-grow : 1; 
}
```

위 처럼 축약형으로 작성할 때 **`flex-basis`값을 생략하면 `auto`가 아닌 0 이 적용**되는데 이 점 주의해야합니다. 





