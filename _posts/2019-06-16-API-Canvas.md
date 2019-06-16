---
layout: post
title:  "[API] CANVAS API "
date:   2019-06-16 18:06:00
author: 한만섭
categories: api
tags: API canvas context 
---

> ## 시작하기 전에 
Canvas 태그를 예전 프로젝트 때 chart를 그릴 곳을 할당할 때 사용했었는데 그때는 아무것도 모르고 사용했는데, 이번에 그림판을 제작해보면서 공부를 
해보려고 합니다. 잠깐 사용해본 결과 굉장히 다양한 것을 제공하는 것 같습니다.  
　  

***

　  

> ### canvas 만들기 
  
  캔버스에 그림을 그리려면 우선 캔버스를 준비해야겠죠. html 내에서 캔버스태그를 만들어줍니다.
  
  ```javascript
  <canvas id="jsCanvas" class="canvas"></canvas>
  ```
  
  하지만, 캔버스를 만들었다고 그림을 그릴 수는 없습니다. 우리가 보는 css의 크기가 아닌 캔버스 객체의 크기를 정해줘야 합니다. 이렇게 해야 캔버스에
  그림을 그릴 수 있습니다.  
  
  * 캔버스 크기 설정 
  
  ```javascript
  canvas.width = 500;
  canvas.height = 500;
  ```
  
  * 컨텍스트 객체 만들기
    캔버스에 그림을 그리는 것은 픽셀을 사용해야 하기 때문에 context객체를 만들어서 그 안의 메소드를 이용해 그림을 그려야 합니다. 그렇기 떄문에 
    컨텍스트 객체를 만드는 것은 필수입니다.  
    
    ```javascript
    ctx = canvas.getContext('2d');
    ```
    
    
  * 컨텍스트 경로 만들기 - **beginPath()**
    여기서는 canvas API에 대해서만 다룰 것이기 때문에 마우스이벤트에 대해서는 생략하도록 하겠습니다.  
    
    ```javascript
    function onMouseMove(event){
    const x = event.offsetX;
    const y = event.offsetY;
    if(!painting){
        ctx.beginPath();
        ctx.moveTo(x,y);
    }else {
        ctx.lineTo(x,y);
        ctx.stroke();
        }
    }
    ```
    
    위와 같이 움직일 때 x,y좌표를 알아내서 그림을 그리는데 `ctx.beginPath();`부분이 이해가 잘 가지 않았습니다. [MDN 참고하기](https://developer.mozilla.org/ko/docs/Web/API/CanvasRenderingContext2D/beginPath)  
    `beginPath()`는 그림을 그리기 시작하겠다는 뜻을 의미합니다. 
    
    
    
  * 컨텍스트 좌표 움직이기 - **moveTo(x,y)**
    캔버스 위에서 마우스를 움직이면 컨텍스트가 가르키고 있는 좌표를 그에 따라 변경시켜줘야 합니다. 왜냐하면 컨텍스트가 그리는 방식은 
    좌표를 이동하고 **stroke()** 혹은 **fill()**과 같은 메소드로 그림을 그리는 것이기 때문입니다. 그래서 아래 코드에서 마우스가 움직일 때 
    
    ```javascript
    function onMouseMove(event){
    const x = event.offsetX;
    const y = event.offsetY;
    if(!painting){
        ctx.beginPath();
        ctx.moveTo(x,y);
    }else {
        ctx.lineTo(x,y);
        ctx.stroke();
        }
    }
    ```
  
  * 선을 그리기 - **lineTo(x,y)**
    x,y좌표로 선을 그려주는 메소드 이다. 이전 MoveTo를 기준으로 그려주는 것 같은데, MoveTo를 하지 않더라도 그려져서 정확히 어떻게 그리는지는 더 
    알아봐야 할 것 같다. 
    
  * 선을 색칠하기 **stroke**
    현재 strkeStyle을 기준으로 지금까지의 경로를 칠해주는 메소드 
