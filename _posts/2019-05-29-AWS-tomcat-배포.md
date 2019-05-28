---
layout: post
title:  "[AWS] ubunto tomcat8에 war배포하기"
date:   2019-05-28 22:28:59
author: 한만섭
categories: Html/css
tags:AWS tomcat8 war
---


## AWS란?

`AWS(amazon web Service)` 란 아마존에서 제공하는 클라우드서비스이다.  
######왜 개발자들은 AWS를 이용하는 것일까? 
웹 프로젝트를 제작하면 서버를 필요로한다. 내가 알기론 예전엔 컴퓨터를 이용해 직접 서버를 운영했는데 비용적인 즉면 , 안정성을 고려해서 클라우스 서버를
운영하는 것 같다.


## 시스템 구성 

정확한 내용이 아니고 독학하며 생각한 내용을 이해하기 쉽게 정리한 것이기 때문에 대부분 정확하지 않을 것임.

크게 `클라우드 컴퓨터(AWS)` 와 `remote git` 과 `war`파일이 필요한 것 같다.  
1. `AWS`에 `linux(인스턴스)` 생성하기  
2. `linux`에 `java` `jdk` `tomcat` 버전을 알맞게 설치하기 
3. legarcy 프로젝트라면 war파일을 `linux`의 tomcat에 넣고 tomcat start
4. boot 프로젝트라면 원격 git 에서 `linux`로 clone 한 후에 mvvn을 통해서 jar파일로 변환하고 실행  

대략적으로 이런 흐름인 것 같다. (아직 DB를 어디에 올려야 하는지는 모름)

## ubuntu tomcat에 war파일 배포하기 

###### 1.인스턴스를 생성해야한다. 
![image](https://user-images.githubusercontent.com/46010705/58495735-29af6380-81b3-11e9-92e9-873419aec222.png)
![image](https://user-images.githubusercontent.com/46010705/58495769-4350ab00-81b3-11e9-9281-588f90cf2fa7.png)
![image](https://user-images.githubusercontent.com/46010705/58495818-6b400e80-81b3-11e9-8074-44101b2f837d.png)
보안 설정은 중요하다. tomcat을 사용할 경우 사용자 지정 TCP 8080포트를 열어주어야 한다. 
위치 무관으로 한 이유는 개발하는 컴퓨터가 바뀔경우 다른 ip도 허용되어야 하기 때문이다. 실제 할때는 ip를 지정해주는 방식 사용하자 linㅣ
`linux`를 사용할떄는 `SSH`로 하면된다 . 


###### 2. 인스턴스에 접속하기 

필자는 [xshell](https://www.netsarang.com/ko/xshell/)을 사용했다.  
실행시켜서 새로만들기를 누르면 아래와 같은 이미지를 볼 수 있다. 
![image](https://user-images.githubusercontent.com/46010705/58496048-ffaa7100-81b3-11e9-85a6-4e6ff414bbf7.png)
![image](https://user-images.githubusercontent.com/46010705/58496153-3ed8c200-81b4-11e9-8f79-a992a61f8b5e.png)
위에 있는 ip주소를 호스트에 넣으면 된다. 
![image](https://user-images.githubusercontent.com/46010705/58496190-5c0d9080-81b4-11e9-893a-1e8f5fe05d2f.png)
위와 같이 하고 생성해주면 된다. 




