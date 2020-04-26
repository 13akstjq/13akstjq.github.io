---
layout: post
title:  "[GitBlog] 깃헙 지킬(jekyll)블로그 사이트 favicon설정하기   "
date:   2019-06-09 17:46:00
author: 한만섭
categories: gitblog
tags: favicon githubio
---

* TOC
{:toc}
## 시작하기전에... 
이번 favicon 수정하면서 우선 사이트 옆에 있는 이미지가 `favicon`이라는 것도 처음 알았다,,, 분명 `config.yml`에서 logo경로를 수정했으니 변해야 한다
고 생각했는데, 알고보니 `assets/icons/`에 각종 아이콘들이 있었다.. 그래서 이걸 다 수정해줘야 하나 고민하고 찾아본 결과, 이미지를 넣으면 favicon
으로 변환해주는 사이트가 있었다.  

그리고 git에 폴더를 잘못만들어서 덕분에 `git Bash`를 사용해서 폴더를 삭제하는 방법도 공부했다.  


　  

### favicon으로 사용할 이미지 정하기  
나같은 경우는 직접 만든 이미지를 사용했다. 이미지를 [favicon변환 사이트](https://www.favicon-generator.org/)에서 변환시켜주면 된다.  
변환하면 아래와 같은 화면이 나온다.  
![image](https://user-images.githubusercontent.com/46010705/59157045-43b84280-8adf-11e9-831c-296848cb0667.png)  
다운로드를 받고 아래 코드를 복사한다. 아래 코드를 gitblog에서 초기화하는 부분에 넣어주면 된다.  
다운받은 파일을 압축풀고 git `/assets/`에 업로드해준다.  



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





### 파일경로 정해주기  

나같은 경우는 `_includes/head.html`에 아래와 같이 미리 정해져 있는 경로가 있었다. ( 지킬테마를 사용한 블로그이기 때문에)
![image](https://user-images.githubusercontent.com/46010705/59157076-a8739d00-8adf-11e9-89a0-58fb5a784a69.png)  
기존의 icon경로를 모두 새로 업로드한 icon경로로 바꿔주면 끝난다.  




https://www.favicon-generator.org/
