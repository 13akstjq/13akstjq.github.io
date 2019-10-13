---
layout: post
title:  "[BlockCar][컴포넌트 스타일] styled-components"
date:   2019-10-12-17:28:00
author: 한만섭
categories: blockcar
tags: blockcar styled-components
---



* TOC
{:toc}
![image](https://user-images.githubusercontent.com/46010705/66698135-425d8780-ed16-11e9-9fcf-633ab205f3fb.png)

***



### Q. 왜 styled-components를 사용했나요?

- 태그에 컴포넌트처럼 이름을 지정해서 사용할 수 있고 `className`을 사용하지 않는것이 직관적이라고 생각했습니다. 

  

***



### Q. 어떻게 사용했나요 ?



#### TypeScript와 함께 사용하기 

두꺼운 글씨를 보여주는 컴포넌트를 제작할 때 글씨의 크기를 styled-components에 넘겨주어서 state에 따라 style을 지정해줄 수 있도록 사용했습니다.  

`src/Components/Commons/FatText.tsx`

```tsx
import React from "react";
import styled from "styled-components";

const Text = styled.span<{ size: number }>`
  font-weight: 600;
  font-size: ${props => props.size}px;
`;

interface IFatTextProps {
  size: number;
  text: string;
}

const FatText: React.FunctionComponent<IFatTextProps> = ({ size, text }) => (
  <Text size={size}>{text}</Text>
);
export default FatText;

```



#### keyframe을 사용한 애니메이션 

로딩 화면에 돌아갈 스피너 애니메이션기능을 위해 styled-components의 `keyframes`를 사용했습니다.  

`src/Components/Commons/Loader.tsx`

```
import React from "react";
import styled, { keyframes } from "styled-components";
import { Spinner } from "../Icons/Commons";

const Wrapper = styled.div`
  width: 100%;
  min-height: 60vh;
  display: flex;
  justify-content: center;
  align-items: center;
`;

const rotate = keyframes`
  from {
    transform : rotate(0deg);
  }to {
    transform : rotate(360deg);
  }
`;

const SpinnerContainer = styled.div`
  animation: ${rotate} 2s infinite;
`;
const Loader = () => {
  return (
    <Wrapper>
      <SpinnerContainer>
        <Spinner></Spinner>
      </SpinnerContainer>
    </Wrapper>
  );
};

export default Loader;

```



***



### Q. styled-components로 직접 개발해보고 느낀 점은 무엇인가요?

- scss와 styled-components를 비교해보기 위해서 직접 scss를 셋팅해보았었는데, `css-loader`와 같은 추가적인 모듈이 필요한 scss와 다르게 npm설치를 통해서 쉽게 사용할 수 있는 것에 매력을 느꼈습니다. 
- presenter에 보여주고 싶은 태그가 많아질 경우 div태그로 보여지는 것보다 기능에 맞는 이름으로 명시해놓으니 나중에 코드를 보아도 헷갈리지 않았습니다. 
- 각 태그마다 styled-components를 만들기 때문에 무분별하게 만들 경우 코드의 길이가 심하게 길어지는 경우도 발생했습니다. 
- 공유할 수 있는 style에 대해서는 설계를 통해서 효율적으로 작성할 수 있도록 개발해야할 것 같습니다.  