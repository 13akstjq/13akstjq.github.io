---
layout: post
title:  "Copy link to slack(1) - 크롬확장프로그램에서 슬랙으로 메세지 보내기"
date:   2019-09-06-20:21:00
author: 한만섭
categories: chrome-extension
tags: google chrome extension copy link to slack

---





* TOC
{:toc}




## 개발할 내용 

저번 포스팅에서는 메세지를 보내는 부분까지 작성을 했는데, 오늘은 현재 Token에 저장된 **Workspace**의 channel list를 불러와 선택한 channel로 현재 URL주소를 보내주는 부분을 개발해보고 나만의 크롬 확장프로그램으로 등록하는 부분까지 알려드리도록 하겠습니다.  



## 0. chrome extension에서 test하기 

확장프로그램을 만들기 위해서는 업로드를 해서 테스트를 하면서 개발할 수 있습니다. 코드가 수정될 때마다 업로드 하는 것이 아니라 새로고침만 해준다면 수정된 부분이 반영되기 때문에 개발 단계에서 굉장히 편리합니다.  

![Honeycam 2019-09-07 21-38-52](../../../../assets/image/Honeycam 2019-09-07 21-38-52.gif)

글로 설명하는 것보다 영상으로 설명드리는게 더 이해가 잘 되실 것 네요! 



## 1. 슬랙 채널 리스트 가져오기 

저는 아래의 API를 사용해서 제 채널 리스트를 가져오려고 합니다.  

![1567857589706](../../../../assets/image/1567857589706.png)

문서를 살펴보도록 하겠습니다.  

![1567857634637](../../../../assets/image/1567857634637.png)

![1567857649410](../../../../assets/image/1567857649410.png)

위 두가지 부분을 보고 코드를 작성할 수 있을 것 같습니다.  



```js
// 현재 Token에 해당하는 workspace의 channel list를 불러오는 메소드
const getChannelList = () => {
  $.post({
    url: `https://cors-anywhere.herokuapp.com/https://slack.com/api/channels.list?token=${WEB_TOKEN}`,
    type: "GET",
    beforeSend: function(xhr) {
      xhr.setRequestHeader("Content-type", "	application/x-www-form-urlencoded");
    },
    success: ({ channels }) => {
    
    },
    error: error => {
      console.log(error);
    }
  });
};

```

이전에 Slack API 내용을 포스팅 했던 것과 달라진 점은 아래 부분인데요.

```js
  beforeSend: function(xhr) {
      xhr.setRequestHeader("Content-type", "	application/x-www-form-urlencoded");
    },
```

이 부분은 요청을 보내기전의 전처리 과정에 해당됩니다. 보내기 전에 **xhr**(xml HTTP Request)의 **Header**에 **Content-type**을 넣어주는 부분입니다. jQuery의 ajax를 사용하면 기존의 axios혹은 fetch에서 header를 다루는 방식과는 약간 다릅니다. 결과적으로 이렇게 작성하면 header를 담아서 요청을 보낼 수 있습니다.   



요청 결과를 console로 확인해보면 아래와 같습니다.  

![1567858207587](../../../../assets/image/1567858207587.png)

이제는 응답받은 데이터를 가공해서 보여주는 작업을 해보도록 하겠습니다.  



### 1.1 채널리스트 데이터 가공하기 

채널 리스트를 확장프로그램 버튼을 클릭했을 때 나타나게 해야하기 때문에 별도의 처리가 필요했습니다.  

![1567858273592](../../../../assets/image/1567858273592.png)

이 부분은 Vanilla JS로 작업하는 부분인데, 여기서 제대로 알지 못했던 부분이 있어서 MDN을 참고해서 정리해보려고 합니다.  

### 1.1.1 document.createElement()

이 것을 사용한 이유는 요청한 채널리스트정보로 **li**태그를 만들어야 했기 때문에 사용했습니다.  argument로 tag의 이름을 넣어주면 태그를 return해줍니다. 

```js
const channelItem = document.createElement("li");
        channelItem.id = channel.id;
        channelItem.innerText = `# ${channel.name}`;
```

위와 같이 작성하면 **li**태그를 만들고 id를 부여하고 text를 channel의 name으로 하겠다는 코드입니다. 그 다음으로는 **ul**태그에 li를 추가해야합니다.  



### 1.1.2 Node.appendChild()

[MDN공식사이트](https://developer.mozilla.org/ko/docs/Web/API/Node/appendChild)를 참고해보면 ul태그에 li를 자식 태그로 추가할 때 사용하면 될 것 같습니다.  

```js
channelList.appendChild(channelItem);
```



부분적으로 살펴봤는데, 완성 됐을 때 이미지와 코드를 통해 보여드리겠습니다. 이게 훨씬 이해가 빠를테니,,,

```js
// 현재 Token에 해당하는 workspace의 channel list를 불러오는 메소드
const getChannelList = () => {
  $.post({
    url: `https://cors-anywhere.herokuapp.com/https://slack.com/api/channels.list?token=${WEB_TOKEN}`,
    type: "GET",
    beforeSend: function(xhr) {
      xhr.setRequestHeader("Content-type", "	application/x-www-form-urlencoded");
    },
    success: ({ channels }) => {
      console.log(channels);
      channels.forEach(channel => {
        const channelItem = document.createElement("li");
        channelItem.id = channel.id;
        channelItem.innerText = `# ${channel.name}`;
        const sendButton = document.createElement("div");
        sendButton.classList.add("sendButton");
        sendButton.innerHTML = `<svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" fill="rgba(255,2555,255,0.5)" viewBox="0 0 24 24"><path d="M24 0l-6 22-8.129-7.239 7.802-8.234-10.458 7.227-7.215-1.754 24-12zm-15 16.668v7.332l3.258-4.431-3.258-2.901z"/></svg>`;
        sendButton.addEventListener("click", () => sendLinkHandler(channel.id));
        channelItem.appendChild(sendButton);
        channelList.appendChild(channelItem);
        console.log(channelItem);
      });
    },
    error: error => {
      console.log(error);
    }
  });
};
```

중간에 들어간 svg파일은 메세지 전송 버튼을 종이비행기 아이콘으로 사용하기 위해 [iconmonstr](https://iconmonstr.com/?s=send)을 이용했습니다.    

위와 같이 코드를 작성했을 때 결과는 아래 이미지와 같습니다.  

![1567858824193](../../../../assets/image/1567858824193.png)

슬랙 icon과 색상을 사용해봤습니다. 아마 이 확장프로그램은 Token을 사용하기 때문에 공개적으로 업로드는 불가능 할 것 같습니다. 직접 Slack App을 만들어서 제작해서 개인적으로 사용하는 것은 문제가 안될 것 같습니다!!   



***



## 2. 채널 선택시 메세지 보내기 

위에서 종이비행기 버튼을 만들 때 eventListener를 등록한 것을 보셨을 것입니다.  

```js
// 입력받은 channelId에 현재 주소를 메세지로 보내주는 메소드
const sendLinkHandler = channelId => {
  chrome.tabs.executeScript(
    {
      code: `window.location.href`
    },
    result => {
      console.log(result);
      if (result === undefined) {
        window.close();
      } else {
        $.post({
          url:
            "https://cors-anywhere.herokuapp.com/https://slack.com/api/chat.postMessage",
          type: "POST",
          beforeSend: function(xhr) {
            xhr.setRequestHeader("Authorization", `Bearer ${WEB_TOKEN}`);
            xhr.setRequestHeader(
              "Content-type",
              "	application/x-www-form-urlencoded"
            );
          },
          data: {
            text: result[0],
            channel: channelId
          },
          success: data => {
            console.log(data);
            window.close();
          },
          error: error => {
            console.log(error);
            alert("Try Again!!!");
          }
        });
      }
    }
  );
};
```

위 함수를 **click**이벤트 발생시 호출되도록 등록해놓은 것입니다. 위 함수는 channel id를 입력받아 그 채널에 메세지를 보내주는 동작을 합니다.   



***



## 3. 결과 영상

![Honeycam 2019-09-07 21-28-43](../../../../assets/image/Honeycam 2019-09-07 21-28-43.gif)



***



## 4. 후기 

이번 확장 프로그램을 만들면서 API의 POST요청과 GET요청에 대해서 많이 공부할 수 있었고, 문서를 읽는 방법도 약간...?은 익힐 수 있었던 좋은 기회였습니다.  

그리고 무엇보다도 제가 필요한 프로그램을 개발한 것이 굉장히 뿌듯했습니다. 이 방법대로 제작하신다면 이 글을 보신 분들도 더 뛰어난 기능의 확장프로그램을 만들 수 있을 것이라 생각합니다! 