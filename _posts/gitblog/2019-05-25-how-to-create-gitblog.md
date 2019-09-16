---
layout: post
title:  "[GitBlog] jekyll 테마로 git blog 만들기"
date:   2019-05-25 22:28:59
author: 한만섭
categories: gitblog
tags:	gitblog github.io jekyell
---

* TOC
{:toc}


지금까지 네이버 블로그를 통해 개발 과정에서 있었던 이슈나 과정을 기록했었는데, 최근 개발자분과 대화할 수 있는 자리를 통해 github blog를 꼭 했으면 좋겠다는 얘기를 들었다. 그래서 오늘부터 나만의 `git blog`를 제작해보려한다.

구글링을 했을 때 git command로 blog 를 제작하시는 분들이 많은데 필자는 아직 `git command`에 대한 지식이 없기 때문에 
나와 같이 개발에 막 입문한 분들을 위해 `command`를 전혀 모른다는 입장으로 만들어보려한다. 
* * *
## 1.원하는 테마 정하기  

일단은 github 아이디가 있다는 전제 하에 글을 쓰겠다. 우선 `gitblog`를 개설하려면 `github repository`가 있어야한다.
직접 repository를 만들어서 작성해도 되지만 [jekyell](http://jekyllthemes.org/) 에 있는 테마 중에서 용도에 알맞은 테마를 사용하려고 한다. 

내가 사용한 테마는 [centrarium](http://jekyllthemes.org/themes/centrarium/)이라는 테마를 사용했다.  
테마를 고를 때는 위 사진과 같이 `MIT License` 가 있는걸 사용하라고 하던데 이유는 잘 모르겠다.

![image](https://user-images.githubusercontent.com/46010705/58370345-50199880-7f40-11e9-91ec-6c798a58d2b5.png)


## 2.jekyell 테마 가져오기
아무튼, 원하는 테마를 골랐다면 이제 그 테마를 내 git repository에 fork해야한다.(fork는 import라고 생각하면 될거같음)
![image](https://user-images.githubusercontent.com/46010705/58370374-bc949780-7f40-11e9-9b30-6fcd9fa731a2.png)

hompage를 누르면 

![image](https://user-images.githubusercontent.com/46010705/58370379-d03ffe00-7f40-11e9-9621-bd2a2c296fac.png)

위와 같은 창이 뜨는데 여러 개발자 분들이 만들어주신 소스이다. 이것을 내 git으로 가져가야하기 때문에 우측 상단의 fork를 눌러준다. 

![image](https://user-images.githubusercontent.com/46010705/58370402-15fcc680-7f41-11e9-8455-61a03ff28fba.png)

fork를 하게 되면 위와 같이 자신의 repository에 추가가 된다.   



<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<!-- 수평 디스플레이 광고 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="4963641784"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>



## 3.추가 설정 
이제 잘 만들어진 theme를 가져왔으니 내 것으로 사용할 차례이다. 

![image](https://user-images.githubusercontent.com/46010705/58370427-6aa04180-7f41-11e9-85c1-a5a7cd407c10.png)

setting메뉴로 가서 repository의 이름을 git닉네임.github.io로 바꿔준다. 

그다음에, url주소를 변경해주어야 한다. 
'config.yml'로 가서 코드를 수정해주어야 한다. 

baseurl: "" 

url: "http://13akstjq.github.io"


위의 내용을 추가했다면 
url의 주소로 블로그를 이용할 수 있다. 





