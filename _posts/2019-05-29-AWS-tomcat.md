---
layout: post
title:  "[AWS] ubunto tomcat8에 war배포하기"
date:   2019-05-29 08:28:59
author: 한만섭
categories: aws
tags:AWS tomcat8 war
---


## AWS란?

`AWS(amazon web Service)` 란 아마존에서 제공하는 클라우드서비스이다.  
##### 왜 개발자들은 AWS를 이용하는 것일까? 
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

### 1.인스턴스를 생성해야한다. 
![image](https://user-images.githubusercontent.com/46010705/58495735-29af6380-81b3-11e9-92e9-873419aec222.png)
![image](https://user-images.githubusercontent.com/46010705/58495769-4350ab00-81b3-11e9-9281-588f90cf2fa7.png)
![image](https://user-images.githubusercontent.com/46010705/58495818-6b400e80-81b3-11e9-8074-44101b2f837d.png)  
보안 설정은 중요하다. tomcat을 사용할 경우 사용자 지정 TCP 8080포트를 열어주어야 한다. 
위치 무관으로 한 이유는 개발하는 컴퓨터가 바뀔경우 다른 ip도 허용되어야 하기 때문이다. 실제 할때는 ip를 지정해주는 방식 사용하자 linㅣ
`linux`를 사용할떄는 `SSH`로 하면된다 . 



### 2. 인스턴스에 접속하기 

필자는 [xshell](https://www.netsarang.com/ko/xshell/)을 사용했다.  
실행시켜서 새로만들기를 누르면 아래와 같은 이미지를 볼 수 있다. 
![image](https://user-images.githubusercontent.com/46010705/58496048-ffaa7100-81b3-11e9-85a6-4e6ff414bbf7.png)
![image](https://user-images.githubusercontent.com/46010705/58496153-3ed8c200-81b4-11e9-8f79-a992a61f8b5e.png)  
위에 있는 ip주소를 호스트에 넣으면 된다.  


![image](https://user-images.githubusercontent.com/46010705/58496190-5c0d9080-81b4-11e9-893a-1e8f5fe05d2f.png)  


위와 같이 하고 생성해주면 된다. 

### 3. 인스턴스에 java jdk 설치하기 

필자가 아직 git command 와 linux command에 익숙하지가 않아서 command 공부는 추가로 해서 업로드 하려고한다...

일단 명령어 마다 `sudo`를 붙히지 않을 거라면 `sudo su root`명령어를 통해 `root`로 접속해야한다.  
프로젝트에 맞는 jdk를 설치해야하는데 나는 zulu8을 사용했기때문에 zulu8, jre, tomcat8을 설치하려고 한다.  


Azul의 public key등록
```
$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 0xB1998361219BD9C9
```

Azul package에 대한 정보를 APT저장소에 등록
```
$ sudo apt-add-repository 'deb http://repos.azulsystems.com/ubuntu stable main'
```

APT의 가용한 패키지들의 정보 update
```
$ sudo apt update
```

```
$ sudo apt install zulu-11
$ java -version
```
```
$ java -version
openjdk version "1.8.0_212"
OpenJDK Runtime Environment (build 1.8.0_212-8u212-b03-0ubuntu1.18.04.1-b03)
OpenJDK 64-Bit Server VM (build 25.212-b03, mixed mode)
```

java 설치 확인 
```
$ which javac
/usr/bin/javac
```
```
$ readlink -f /usr/bin/javac
```

java 환경 변수 설정하기  
환경 변수를 설정하기 위해서는 profile 폴더로 이동해야한다. 
```
nano /etc/profile
```
root에서 하는것이 아니면 sudo를 붙혀야함  
```
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export PATH=$JAVA_HOME/bin/:$PATH
export CLASS_PATH=$JAVA_HOME/lib/:$CLASS_PATH
```
추가하고 ctrl + x / y / enter 누르기  

리로드를 해야한다. 
```
$ source /etc/profile
``` 

환경변수를 학인하는 명령어 
```
echo $JAVA_HOME
$JAVA_HOME/bin/javac -version
```


### 4. 인스턴스에 tomcat8 설치하기 

```
apt-get install tomcat8
```
tomcat 버전확인하는 명령어 
```
/usr/share/tomcat8/bin/version.sh
```

원래는 방화벽을 변경해주어야한다.(tomcat은 8080포트를 사용하기 때문) 하지만, 처음에 인스턴스를 생성할 때 보안그룹에 추가하였기 때문에 넘어가도됨 

tomcat 실행시키는 명령어 
```
service tomcat8 start
```

tomcat을 실행시켰다면 `인스턴스ip:8080'으로 접속해서 확인해보자 
![image](https://user-images.githubusercontent.com/46010705/58497528-1acab000-81b7-11e9-9a1f-20b220cfc71b.png)  


이런 화면이 나오면 성공적으로 설치된 것이다. 


### 5. 프로젝트 옮기기 

인스턴스에 필요한 것을 모두 설치했다. 그러면 이제 서버에 올려놓고 싶은 프로젝트를 war형식으로 넘겨줄 차례이다.   
우선 프로젝트를 war로 빼놓는다.   
war파일은 노트북이나 데스크탑같은 개인 개발환경에 있을 것이다. 이것을 인스턴스인 linux에 옮기려면 [filezilla](https://filezilla-project.org)가 필요하다.

![image](https://user-images.githubusercontent.com/46010705/58497813-b8be7a80-81b7-11e9-9bbd-5daecd7ee681.png)


호스트는 인스턴스 ip를 입력하면 된다.  

그다음에 war파일을 드래그앤 드롭으로 넣으면 된다.  
넣어놓고 tomcat이 있는 경로로 mv 명령어를 통해서 옮기면 되기 때문이다. 

이제 여기서 권한 설정으로 골치아팠던 부분이다. 
```
ls -al
``` 

아마 home에 war파일이 있을텐데 이것을 `/var/lib/tomcat8/webapps/` 로 옮겨야 한다.
하지만 권한과 소유가 설정되어 있지 않았다.
```
drwxrwxr-x 4 tomcat8 tomcat8     4096 May 28 14:55 .
drwxr-xr-x 5 root    root        4096 May 28 14:12 ..
drwxr-xr-x 3 tomcat8 tomcat8     4096 May 28 14:10 ROOT
drwxr-x--- 5 tomcat8 tomcat8     4096 May 28 14:55 hmProject
-rwxrwxrwx 1 tomcat8 tomcat8 65357651 May 28 14:35 hmProject.war

```

위와 같이 war가 tomcat의 소유여야한다. 

파일 이동하는 명령어 
```
$ mv hmProject.war /var/lib/tomcat8/webapps/
```

아마 permission denied 가 뜰 것임.


퍼미션 변경하기 
```
chmod [변경될 퍼미션값] [변경할 파일]
```

소유자 변경하기 
```
chown [변경할 소유자],[변경할 그룹] [변경할 파일]
```

### 6. tomcat command


tomcat start/stop
```
$ sudo service tomcat7 stop
$ sudo service tomcat7 start
```

tomcat 의 구동상황
```
$ systemctl status tomcat7.service
```

테스트
```
http://서버ip:8080/SocketTest(프로젝트명)/파일이나 url
```

[java설치 참고 사이트](https://minimi22.tistory.com/5?category=768638)
[zulu설치 참고 사이트](http://geeks.underslow.com/36/)
[tomcat 배포참고 사이트](http://blog.naver.com/PostView.nhn?blogId=loverman85&logNo=221073024524&categoryNo=35&parentCategoryNo=0&viewDate=&currentPage=1&postListTopCurrentPage=1&from=postView)
[권한 command 참고 사이트](https://conory.com/blog/19194)

