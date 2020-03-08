---
layout: post
title:  "[API] 지킬(jekyll)블로그에 Google Analytics Reporting API 사용해서 방문자 기능 추가하기"
date:   2020-03-08 17:46:00
author: 한만섭
categories: api
tags: GoogleAnalytics API jekyllBlog githubPages
---

* TOC
{:toc}
오늘은 많은 개발자 분들이 사용하고 계시는 지킬(jekyll)블로그 gihubPages라고도 불리우는 블로그에 방문자를 사용할 수 있도록 googleAnalytics API를 사용해서 해당 사이트의 방문 횟수를 알아내는 방법을 정리하도록 하겠습니다. 

## Google Analytics API

  API의 사용법을 알아보기 위해 [Google Analytics Reporting API v4](https://developers.google.com/analytics/devguides/reporting/core/v4)를 들어가봅니다. 

![image](https://user-images.githubusercontent.com/46010705/76166508-6bd5ee80-61a2-11ea-8a47-b2e45552b5cc.png)

위 사진 처럼 여러 언어를 사용해서 Google Analytics API를 이용할 수 있습니다. 저는 JavaScript를 이용할 것이기 때문에 JavaScript를 눌러줍니다. 

### 1. Enable the API

![image](https://user-images.githubusercontent.com/46010705/76166541-a5a6f500-61a2-11ea-8954-9653d3e694e0.png)

자바스크립트를 누르면 위와 같은 순서로 credential을 create해주라는 설명이 나옵니다. 차근차근 따라해보겠습니다. 

![image](https://user-images.githubusercontent.com/46010705/76166615-2534c400-61a3-11ea-918d-dffe6ea2e234.png)

위의 네모를 눌러 Google API를 사용할 새로운 프로젝트를 생성해줍니다. 

![image](https://user-images.githubusercontent.com/46010705/76166640-61682480-61a3-11ea-82a3-aa36fb5701c1.png)

생성된 프로젝트에 사용자 인증정보를 작성해줍니다. URL은 실제 API 사용할 URL주소를 사용해야합니다. 우측에 있는 client_id는 밑에서 사용할 것이기 때문에 복사해둡니다. 

### 2. Setup the sample

![image](https://user-images.githubusercontent.com/46010705/76166665-9e341b80-61a3-11ea-83b1-1443128bd7af.png)

아래 코드의 client_id부분과 view_id부분을 여러분의 id로 변경해줘야합니다. `client_id`는 아까 위에서 복사했던 것을 사용하면 됩니다. View_id는 위의 화면에서 `Account_Explorer`를 클릭하면 확인할 수 있습니다. 

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>Hello Analytics Reporting API V4</title>
  <meta name="google-signin-client_id" content="<REPLACE_WITH_CLIENT_ID>">
  <meta name="google-signin-scope" content="https://www.googleapis.com/auth/analytics.readonly">
</head>
<body>

<h1>Hello Analytics Reporting API V4</h1>

<!-- The Sign-in button. This will run `queryReports()` on success. -->
<p class="g-signin2" data-onsuccess="queryReports"></p>

<!-- The API response will be printed here. -->
<textarea cols="80" rows="20" id="query-output"></textarea>

<script>
  // Replace with your view ID.
  var VIEW_ID = '<REPLACE_WITH_VIEW_ID>';

  // Query the API and print the results to the page.
  function queryReports() {
    gapi.client.request({
      path: '/v4/reports:batchGet',
      root: 'https://analyticsreporting.googleapis.com/',
      method: 'POST',
      body: {
        reportRequests: [
          {
            viewId: VIEW_ID,
            dateRanges: [
              {
                startDate: '7daysAgo',
                endDate: 'today'
              }
            ],
            metrics: [
              {
                expression: 'ga:sessions'
              }
            ]
          }
        ]
      }
    }).then(displayResults, console.error.bind(console));
  }

  function displayResults(response) {
    var formattedJson = JSON.stringify(response.result, null, 2);
    document.getElementById('query-output').value = formattedJson;
  }
</script>

<!-- Load the JavaScript API client and Sign-in library. -->
<script src="https://apis.google.com/js/client:platform.js"></script>

</body>
</html>

```

### 3. Run the sample

여기 까지 했다면 이제 여러분의 용도에 맞게 사용하시면 됩니다. 

```js
dateRanges: [
  {
    startDate: "2019-05-25",
    endDate: "today"
  }
],
  metrics: [
    {
      expression: "ga:users"
    }
  ]
```

저 같은 경우에는 블로그의 생성 날짜인 2019-05-25부터 오늘 까지의 users를 요청해서 사용했습니다. 

![image](https://user-images.githubusercontent.com/46010705/76166775-72656580-61a4-11ea-80aa-10748ed4b059.png)

그렇게 할 경우 위와 같은 결과 창을 얻을 수 있습니다. 저는 4,270이라는 많은 분들이 방문 해주셨네요. 



### 4. 활용 방안 

실제 지킬 블로그에 적용하는 부분까지는 작성하지 않았지만, Google Analytics API를 요청해서 필요한 정보를 받는 서버를 가지고 있다면 지킬(jekyll) 블로그에서 ajax요청을 통해서 블로그 전체 방문자 수 혹은 게시물 별 방문자 수를 보여줄 수 있을 것 같습니다. 

