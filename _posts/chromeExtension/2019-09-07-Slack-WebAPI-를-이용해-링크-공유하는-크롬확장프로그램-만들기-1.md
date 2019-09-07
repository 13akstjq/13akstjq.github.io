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

우선 크롬 확장 프로그램에서 슬랙으로 메세지가 보내지는 것을 확인한 후에 더 발전시킬 수 있기 때문에 크롬 확장 프로그램에서 슬랙에 메세지를 보내는 부분까지 만들어보도록 하겠습니다.  이 게시물은 **slack Web API**를 알고 있는 상태에서 작성하는 것이기 때문에 API에 대한 설명은 하지 않겠습니다.  

***



## 1. 첫번째 시도 - incoming webhook API

**Slack API**에서 메세지를 보내는 방법에는 **incoming webhook API** 와 **chat.postMessage**가 있는데 우선 **incoming webhookk API**를 사용해서 개발해보도록 하겠습니다.  



### 1-1. incoming webhook API + fetch 

- 코드 

```js

      fetch(
        "https://cors-anywhere.herokuapp.com/" +
          "https://hooks.slack.com/services/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",
        {
          method: "post",
          mode: "cors",
          data: {
            text: result[0],
            channel: "CN1GMB1TJ"
          },
          headers: {
            "Content-type": "application/x-www-form-urlencoded"
          }
        }
      ).then(res => console.log(res));
```

- 실행 결과 

  bad request 결과가 나왔습니다. 

![1567839428534](../../../../assets/image/1567839428534.png)



### 1-2. incoming webhook API + ajax

ajax를 사용할 때는 chrome extension에서는 cdn이 사용 불가이기 때문에 직접 다운로드를 해서 사용해야 합니다. [jquery사이트](https://blog.jquery.com/2019/05/01/jquery-3-4-1-triggering-focus-events-in-ie-and-finding-root-elements-in-ios-10/)에서 최신 버전으로 다운 받아서 사용했습니다.   



- 코드

```js
// //incoming webhook api + ajax
      $.post({
        url:
          "https://cors-anywhere.herokuapp.com/https://hooks.slack.com/services/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",
        type: "POST",
        beforeSend: function(xhr) {
          xhr.setRequestHeader(
            "Content-type",
            "	application/x-www-form-urlencoded"
          );
        },
        data: {
          text: result[0],
          channel: "CN1GMB1TJ"
        },
        success: data => {
          console.log(data);
        },
        error: error => {
          console.log(error);
        }
      });
```

- 결과

  이 방법도 bad request 가 나옵니다. 

![1567839727678](../../../../assets/image/1567839727678.png)



***



## 2. Web API 

두번째로는 Web APi의 token을 사용해서 개발하는 방법을 해보겠습니다. 



### 2-1. Web API + fetch 

- 코드 

```js
 fetch(
        "https://cors-anywhere.herokuapp.com/https://slack.com/api/chat.postMessage",
        {
          method: "post",
          mode: "cors",
          data: {
            text: result[0],
            channel: "CN1GMB1TJ"
          },
          headers: {
            "Content-type": "application/x-www-form-urlencoded",
            Authorization:
              "Bearer xoxb-738105262323-750907850116-e60lMiph0eNPmTZKtR1ENyUj"
          }
        }
      ).then(res => console.log(res));
```

- 실행 결과 

![1567839919455](../../../../assets/image/1567839919455.png)



### 2-2. Web API + ajax

- 코드

```js
 $.post({
        url:
          "https://cors-anywhere.herokuapp.com/https://slack.com/api/chat.postMessage",
        type: "POST",
        beforeSend: function(xhr) {
          xhr.setRequestHeader(
            "Authorization",
            "Bearer xoxb-738105262323-750907850116-e60lMiph0eNPmTZKtR1ENyUj"
          );
          xhr.setRequestHeader(
            "Content-type",
            "	application/x-www-form-urlencoded"
          );
        },
        data: {
          text: result[0],
          channel: "CN1GMB1TJ"
        },
        success: data => {
          console.log(data);
        },
        error: error => {
          console.log(error);
        }
      });
```

- 실행 결과 

![1567840066985](../../../../assets/image/1567840066985.png)

***



***



## 3. 결론 

결과를 살펴보니 **Web API + ajax**를 사용해서 호출했을 경우에만 동작하는 것을 확인할 수 있습니다. 다음 포스팅에서는 보낼 채널을 선택할 수 있도록 하는 기능을 추가하려고 합니다.  

> ###  



