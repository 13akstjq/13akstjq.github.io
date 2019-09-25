---

layout: post
title:  "[debug] form 사용시 submit이 되지 않는 이슈 "
date:   2019-09-25-13:06:00
author: 한만섭
categories: debug
tags: debug typescript form submit
---


* TOC
{:toc}


### 이슈 

form을 사용해서 로그인을 구현하던 중 발생한 이슈 

```html
<form>
	<input type="text"/>
    <input type="text"/>
</form>
```

위와 같이 작성했더니 엔터키를 입력해도 form이 제출되지 않는 이슈가 발생했음. 



### 해결 

form 에서는 button이라는 태그를 사용하지 않으면 input태그가 여러개일 경우에 enter를 쳐도 제출이 되지 않는 문제가 있었음. 아래와 같이 버튼을 사용해주면 됨.  



```html
<form>
        <input type="text"/>
        <input type="text"/>
        <button>제출</button>
      </form>
```











