

```
layout: post
title:  "[chromeExtension] 크롬 확장프로그램 등록 방법 "
date:   2019-07-22 20:21:00
author: 한만섭
categories: chromeextension
tags: google chrome extension
```







> ### 확장프로그램 등록하기 

![1563802204425](../../../../assets/image/1563802204425.png)





> ### chrome storage

* storage에 데이터 저장 

```js
chrome.storage.sync.set({ userData: user });
```

* storage에서 데이터 가져오기 

```js
chrome.storage.sync.get(["userData"], result => {});
```



* 사이트에 code 적용시키기 

```js
 chrome.tabs.executeScript(
    {
      code: 'document.querySelector("body").innerText;'
    },
    function(result) {});
```



> ### manifest.json

```json
{
  "manifest_version": 2,
  "name": "FrequencyOf",
  "description": "My lovely words",
  "version": "1.0",
  "browser_action": {
    "default_icon": "icon.png",
    "default_popup": "popup.html"
  },
  "permissions": ["activeTab", "tabs", "storage"]
}

```

