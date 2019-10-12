---
layout: post
title:  "[BlockCar][design pattern] 프론트엔드 개발자 2명에서 디자인 패턴 적용하며 개발한 후기"
date:   2019-10-10-23:28:00
author: 한만섭
categories: blockcar
tags: blockcar designpattern 
---



* TOC
{:toc}


***



### Q. 어떤 디자인 패턴을 사용해서 개발했나요? 

- React의 가장 기본적인 design pattern인 Container Presenter 패턴을 사용해서 개발했습니다. 



### Q. 왜 Container Presenter방식을 사용했나요?

- 처음에는 싱글파일 컴포넌트 형태로 개발을 했었는데, 컴포넌트의 크기가 커지게 되면 가독성이 떨어지게 됩니다. 
- 평소에 모듈화를 해서 개발하는 편이였습니다. 데이터를 요청하고 처리하는 코드와 데이터를 보여주는 코드가 한 곳에 존재하는 것이 불편하다고 생각했고, 이 것을 해결해줄 수 있는 방식이라고 생각해서 적용했습니다. 



### Q. 어떻게 사용했나요 ?

- 이해가 쉽도록 폴더 트리구조를 통해서 설명드리겠습니다.  

  ![1570718298974](../../../../assets/image/1570718298974.png)

  `Routes`밑에 페이지별로 폴더를 만들어줍니다. 폴더 안에는 `index`, `MainContainer`,`MainPresenter`를 만들어줍니다.  

  `MainPresenter.tsx`

  ```js
  import React from "react";
  
    const MainPresenter =  () => "MainPresenter";
  
  export default MainPresenter;
  
  ```

  

  `MainContainer.tsx`

  ```js
  import React from "react";
  import MainPresenter from './MainPresenter';
  
    const MainContainer =  () => {
    return <MainPresenter/>
    }
  
  export default MainContainer;
  
  ```

  

  `index.tsx`

  ```js
  import React from "react";
  import MainContainer  from './MainContainer';
  
  export default MainContainer;
  ```

  

  위 처럼 코드를 작성해서 데이터 처리 부분과 보여주는 부분을 구분할 수 있도록 작성하면 됩니다.  



### Q. 디자인 패턴을 사용한 결과는 어땟나요? 

- 어떤 디자인 패턴을 사용했는지에 상관 없이 일단 폴더 구조 및 코드가 통일성을 갖고 있었다는 게 좋았습니다. 

- 같은 기능을 작성한다면 팀원들과 거의 동일한 코드를 작성할 수 있었습니다. 이것의 장점은 팀원의 코드를 수정하는데 추가적인 이해나 소통이 필요하지 않는다는 것 입니다.  

- Container Presenter 방식의 장점은 데이터 처리부분과 보여지는 부분이 나뉘어 있다는 것이 좋았습니다. 데이터를 보여주는 부분을 수정하고 싶으면 Presenter파일로 가면 되고, 데이터 요청 및 처리에 관련된 코드를 구현하고 싶다면 Container파일로 가서 코드를 작성하면 되었기 때무입니다.  

- 이 프로젝트는 TypeScript를 사용했기 때문에 Container 에서 Presenter로 Props를 내려보낼 때 일일히 Type을 정해줘야 한다는 불편함이 있었습니다. 하지만, 이것도 TypeScript가 적응되고 나니까 버그를 미리잡아줄 수 있어서 오히려 좋았습니다.  

  