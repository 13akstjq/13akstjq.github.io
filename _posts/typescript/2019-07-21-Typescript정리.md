---
layout: post
title: "[typescript] typescript 정리"
date: 2019-07-21 20:21:00
author: 한만섭
categories: typescript
tags: react typescript styledcomponents
---

- TOC
  
  {:toc}

## 1. 타입스크립트

---

변수가 자유로운 `JavaScript`에서는 이슈를 `Production`단계인 실행시켰을 경우 혹은 실제 배포했을 때 `bug`를 발견하는 경우가 많습니다.

`TypeScript`는 이러한 변수의 `Type`에 관한 `Error`를 실행 및 배포 단계에서 알아차리는 것이 아니라 **개발단계**에서 미리 막아주기 위해 사용됩니다.

자료형 외에도 `Interface`를 사용해서 객체의 type까지 지정해 놓을 수 있다.

?로 `optional value`를 지정할 수 있다.

### 1.1 TypeScript 와 React 같이 사용하기

- typescript를 사용할 프로젝트 만들기

  ```bash
  npx create-react-app typescript-react-demo --typescript
  ```

아래 처럼 더하기 함수를 만들때 `a,b`의 type을 정해주지 않으면 에러가 발생합니다.

![1563719722956](../../../../assets/image/1563719722956.png)

에러를 막기 위해서 type을 지정해줘야 합니다. 아래와 같이 수정하면 에러가 발생하지 않는 것을 확인할 수 있습니다.

![1563720261692](../../../../assets/image/1563720261692.png)

---

### 1.2 라이브러리 사용하기

typescript는 강력하기 때문에 `npm`을 통해서 설치한 것에 대한 정보도 제공해줘야 합니다.

예시를 들기 위해서 제가 좋아하는 `styled-component`를 사용해보려고 합니다.

```bash
 npm add styled-components
```

`index.tsx`에 `styled-components`를 사용해보겠습니다.

`index.tsx`

![1563720635457](../../../../assets/image/1563720635457.png)

위와 같이 `styled-components`를 인식하지 못하는 것을 확인할 수 있습니다.

`typeScript`에서는 리액트 라이브러리에 대한 `type`정의가 되어있지 않기 때문입니다.

하지만 멋진 개발자가 npm에서 많이 사용되는 유명 library에 대해서 `type`정의를 해놓았습니다.......

저희는 그것을 그냥 가져다가 쓰기만하면 됩니다.

[definitly typed](https://github.com/definitelytyped/definitelytyped)

- type형식의 라이브러리 설치하기

  ```bash
  npm add @types/styled-components
  ```

위 명령어를 통해서 `typed`된 `styled-components`를 설치할 수 있습니다.

![1563720940524](../../../../assets/image/1563720940524.png)

설치를 하게 되면 위와 같이 아까의 에러가 발생하지 않는 것을 확인할 수 있습니다.

그렇다면 npm에서 모든 라이브러리에 대해서 `@types`를 사용할 수 있는가? 그렇지 않습니다.

유명 `npm`에 대해서만 적용되기 때문에 사용하는 `npm`이 없을 수도 있습니다. 그럴때는 아래와 같은 rule을 적용시켜서 `typescript`의 에러를 피할 수 있습니다. 또한 자동완성까지 제공해주기 때문에 좀더 편하게 사용할 수 있습니다.

`tsconfig.json`

```json
"noImplicitAny": true
```

![1563721078647](../../../../assets/image/1563721078647.png)





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



### 1.3 TypeScript 와 React 의 State

아래와 같이 `state`를 선언하고 값을 변경하려고 하면 typescript는 component의 `props`와 `state`값이 어떤 것이 있는지 알지 못합니다.

![1563722142417](../../../../assets/image/1563722142417.png)

아래와 같은 방법으로 component에게 `props`와 `state`의 `type`을 알려줄 수 있습니다.

![1563722280283](../../../../assets/image/1563722280283.png)

`State`에 대해서 Interface형식으로 정의를 내려 놓고 `component`에게 제공해주면 됩니다.

![1563722346280](../../../../assets/image/1563722346280.png)

`interface`형태로 `State`의 type을 지정해놓으면 위와 같이 `count`에 마우스를 호버하였을 경우 count가 `number`라는 것을 알려줍니다.

---

### 1.4 react event에 typescript 사용하기

우선 이벤트를 적용해보기 위해 `input`과 `form` 컴포넌트를 만들어 줍니다.

```js
import React from "react";

export const Input: React.FunctionComponent = ({ value, onChange }) => (
  <input value={value} onChange={onChange} />
);

export const Form: React.FunctionComponent = ({ children }) => <form />;
```

![1563728111623](../../../../assets/image/1563728111623.png)

위 사진에서는 `React.FunctionComponent`를 사용하면 `{children}`은 기본적으로 제공되는 `prop`이기 때문에 에러가 발생하지 않는 모습을 볼 수 있습니다.

`value,onChange`는 `interface`를 사용해서 컴포넌트에게 알려줘야 합니다.

```typescript
interface IInputProp {
  value: string;
  onChange: () => void;
}
```

위와 같이 value는 string 이고 onChange는 function인 `interface`를 만들어줍니다.

```typescript
import React from "react";

interface IInputProp {
  value: string;
  onChange: () => void;
}

export const Input: React.FunctionComponent<IInputProp> = ({
  value,
  onChange
}) => <input value={value} onChange={onChange} />;

export const Form: React.FunctionComponent = ({ children }) => <form />;
```

![1563728433708](../../../../assets/image/1563728433708.png)

위와 같이 넣어주게 되면 에러가 발생하지 않는 모습을 볼 수 있습니다.

그 다음에 `App.tsx`에서 `Input,Form`을 사용하게 되면 아래와 같은 에러를 발생시킵니다.

![1563728523271](../../../../assets/image/1563728523271.png)

위 에러는 `IInputProp`에서는 `value`와 `onChange`를 필요로 하는데 넣어주지 않았기 때문에 발생하는 에러입니다. 그러면 넣어주도록 하겠습니다.

- value 추가하기

![1563728660618](../../../../assets/image/1563728660618.png)

위와 같이 `value`를 추가했지만 에러가 발생하는 것을 볼 수 있습니다. 그 이유는 `IState`에는 `value`라는 것이 존재하지 않기 때문입니다. 그래서 사전에 state에 대해서 추가할때 에러를 방지할 수 있는 것입니다.

![1563728984905](../../../../assets/image/1563728984905.png)

위 사진은 `onChange`를 추가한 모습입니다. 하지만 `Input`에서 정의한 `IInputProp`에서는 `onChange`가 어떠한 인자도 받지 않는 함수라고 했지만 `App.tsx`에서 정의한 `onChange`는 `e`라는 이벤트 인자를 받기 때문에 서로 맞지 않아서 오류가 발생하는 모습입니다.

### 1.5 이벤트 type정해주기

위와 같은 이슈를 해결하기 위해 이벤트의 type을 정해주어야 합니다. form의 event다... 아니면 input의 이벤트다...라고 알려줘야 하는 것 입니다.

![1563729375737](../../../../assets/image/1563729375737.png)

위와 같이 추가해주면 `event`의 type을 컴포넌트에게 알려줄 수 있습니다.

```typescript
(e: React.SyntheticEvent<HTMLInputElement>)
```

- children

event와 상관 없는 얘기지만 children에 대해 얘기하고 넘어가겠습니다.

![1563729781640](../../../../assets/image/1563729781640.png)

위와 같이 `Form`안에 `Input`을 사용하게 되면 `Form`태그는 `children` `props`를 사용해서 `form`안에 넣어줄 수 있습니다.

### 1.6 onSubmit 만들기

![1563729970699](../../../../assets/image/1563729970699.png)

onSubmit도 역시 `e`만 쓰게 되면 event의 `type`을 정해주지 않았기 때문에 에러가 발생합니다.

아래와 같이 event의 type을 지정해주어야 합니다.

![1563730150576](../../../../assets/image/1563730150576.png)

위와 같이 추가를 해주게 되면 자동완성까지 가능하게 됩니다.





### 1.7 styled component 에 typescript 적용시키기

[공식사이트](https://www.styled-components.com/docs/api#typescript)

![1563731690825](../../../../assets/image/1563731690825.png)

숫자가 10보다 클 경우 글씨의 색을 빨간색으로 만들고 싶다면 위와 같이 작성하면 됩니다.

styled component의 경우 많이 작성하게 되면 `interface`의 양도 많아지기 때문에 위와 같이 `inline`방식으로 작성하는 것이 편할 것 같습니다.

`Number.tsx`

```typescript
import React from "react";
import styled from "styled-components";

const Container = styled.span<{ isRed: boolean }>``;

interface IProps {
  number: Number;
}

const Number: React.FunctionComponent<IProps> = ({ number }) => (
  <Container isRed={number >= 10 ? true : false}>{number}</Container>
);

export default Number;
```

