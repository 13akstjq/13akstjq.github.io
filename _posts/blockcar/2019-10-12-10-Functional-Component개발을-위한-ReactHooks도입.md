---
layout: post
title:  "[BlockCar] Functional-Component개발을-위한-ReactHooks도입"
date:   2019-10-12-23:28:00
author: 한만섭
categories: blockcar
tags: blockcar Functional Component ReactHooks
---



* TOC
{:toc}
![image](https://user-images.githubusercontent.com/46010705/66696033-22709880-ed03-11e9-8f49-d27648884c22.png)

***



### Q. 왜 React Hooks를 사용했나요?

- 클래스 컴포넌트 사용시 `this`의 스코프에 대한 불편함이 있었습니다.
- 알고리즘을 공부했었기 때문에 함수형 프로그래밍이 더 익숙했었습니다. 

***



### Q. 어떻게 사용했나요 ?



#### 1. useState

- functional 컴포넌트에서는 state를 사용할 수 없기 떄문에 useState로 State를 사용할 수 있습니다.  

  메인 배너에서 보여질 이미지의 index를 담는 변수를 만들기 위해 아래와 같이 사용했습니다. 

  `src/Components/MainBanner.tsx`

  ```js
  import React, { useState, useEffect } from "react";
  import styled from "styled-components";
  import { LeftIcon, RightIcon } from "./Icons/DirectionIcon";
  import bg_2 from "../assets/image/bg_2.png";
  import bg_3 from "../assets/image/bg_3.png";
  import bg_4 from "../assets/image/bg_4.png";
  
  (...스타일 생략...)
  
  
  const MainBanner: React.FunctionComponent = () => {
    const images = [
      bg_2,
      bg_3,
      bg_4,
    ];
    const [selectedImage, setSelectedImage] = useState(0);
    const moveSize = "100vw";
    const slideInterval = 4000;
    const slide = () => {
      if (selectedImage == images.length - 1) {
        setTimeout(() => {
          setSelectedImage(0);
        }, slideInterval);
      } else {
        setTimeout(() => {
          setSelectedImage(selectedImage + 1);
        }, slideInterval);
      }
    };
  
    useEffect(() => {
      slide();
    });
  
    return (
      <Wrapper>
        <ImageListContainer
          selectedImage={selectedImage}
          length={images.length}
          moveSize={moveSize}
        >
          {images.map((image, index) => (
            <Image key={index} moveSize={moveSize} url={image}></Image>
          ))}
        </ImageListContainer>
     
      </Wrapper>
    );
  };
  
  export default MainBanner;
  
  ```



#### 2. useEffect

클래스 컴포넌트에는 컴포넌트가 마운트되었을 때 호출되는`ComponentDidMount`, state가 업데이트 될 때 호출되는 `ComponentWillUpdate`, 컴포넌트가 사라지기 전에 호출되는 `ComponentWillUnMount`와 같은 다양한 라이프 사이클 메소드가 존재합니다.  

하지만 함수형 컴포넌트에서는 이것들이 존재하지 않습니다. 그래서 함수형 컴포넌트에서도 적절한 라이프사이클에 원하는 동작을 할 수 있도록 만들어준 것이 `useEffect`입니다.  

- ComponentDidMount

  아래 코드는 마이페이지 화면에서 페이지가 마운트 될 때 판매신청 목록과 구매신청목록을 불러오는 라이프사이클 훅입니다. 

  `Routes/MyPage/MyPageContainer.tsx`

  ```js
    useEffect(() => {
      salesList();
      purchaseList();
    }, []);
  ```

  

- ComponentWillUpdate

  아래 코드는 메인 베너에서 state가  업데이트될 경우 `slide()`라는 함수를 호출할 수 있도록 작성한 라이프 사이클 훅입니다. 

  `src/Components/MainBanner.tsx`

  ```
    const slide = () => {
      if (selectedImage == images.length - 1) {
        setTimeout(() => {
          setSelectedImage(0);
        }, slideInterval);
      } else {
        setTimeout(() => {
          setSelectedImage(selectedImage + 1);
        }, slideInterval);
      }
    };
  
    useEffect(() => {
      slide();
    });
  ```



#### 3. Hooks의 모듈화 

input태그를 사용하게 될 경우 아래와 같이 매번 코드를 작성해줘야 합니다.  

```js
const [count , setCount] = useState(0);
const onChange = (e) => {
	setCount(e.target.value);
}
return <input value={count} onChange={onChange} /> 
```

매번 value와 onChange를 넣어주는 불편함을 해결하기 위해 `useInput`이라는 파일을 따로 만들어서 input에 사용되는 값을 모듈화 했습니다.  

`Components/Hooks/useInput.tsx`

```tsx
import React, { useState } from "react";

const useInput = (initialState: any) => {
  const [value, setValue] = useState(initialState);

  const onChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    setValue(e.target.value);
  };

  return { value, setValue, onChange };
};

export default useInput;

```

```tsx
// carOwnerInfo
  const carOwnerNameInput = useInput("한싸피");

return <input {...carOwnerInput}> </input>
```

ES6의 spread operator와 함께 사용하면 위와 같이 쉽게 input의 기본 값을 셋팅할 수 있습니다. 



***



### Q. ReactHooks로 직접 개발해보고 느낀 점은 무엇인가요?

- 클래스 컴포넌트를 사용했을 때보다 쉽게 작성할 수 있었고, 코드 길이가 짧아지는 부분에서 좋다고 생각합니다.  
- 하지만 클래스 컴포넌트 방식을 깊게 이해하지 않은 채로 Hooks를 도입하는게 맞는 것인지 고민을 하게 해주었습니다.  
- 라이프 사이클 훅이 클래스 컴포넌트처럼 독립적으로 존재하는 것이 아니라 useEffect 하나만 가지고 사용하기 때문에 발생하는 이슈가 있었습니다. 이와 같이 React Hooks를 사용해서 해결하지 못하는 문제가 발생할 경우에 대해서 대비가 필요할 것 같습니다.  

