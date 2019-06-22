---
layout: post
title:  "[AWS] ubuntu에 spring 프로젝트와 mysql 연결하기  "
date:   2019-05-30 20:00:59
author: 한만섭
categories: aws
tags: aws spring mysql
---


## aws에 spring 프로젝트와 mysql 연동하기

진짜 무지한 실력으로 하나하나 시도하다보니 정말 오래 걸렸다.  

### 시스템 구성 

`aws`에 있는 `tomcat8`에 `war`파일을 올려놓고, 같은 `aws`서버에 `mysql`을 설치하고 설정해주는 흐름이다. 

## 1. mysql 설치하기 
```
  apt-get install mysql-server
```

## 2. mysql 에 로그인하기 
```
  mysql -u root -p
  비밀번호 
```

## 3. aws 에서 인바운드 설정
![image](https://user-images.githubusercontent.com/46010705/58636665-ca289380-832b-11e9-868d-39fac857eda5.png)
  
보안그룹에서 3306포트를 열어주어야 한다.  

![image](https://user-images.githubusercontent.com/46010705/58636903-4b802600-832c-11e9-9595-180dd73cad8c.png)

mysql을 3306포트에서 사용할 수 있도록 해주는 것이다.   

## 4. mysql 외부 접속 허용 설정

mysql을 설정할 수 있는 conf로 가기 
```
 cd /etc/mysql/mysql.conf.d
 vi mysqld.cnf
``` 

- vi파일을 수정하는 방법
i를 누르면 수정가능해짐 
수정한 후에 ctrl + c로 명령모드로 바꿈 
:wq 혹은 :w!를 입력하면 저장됨.

외부 접속 허용 설정해준 후에 mysql 재시작 및 로그인 
```
service mysql restart
mysql -u root -p
```

- root 계정 권한 설정
```
grant all on *.* to 'root'@'3.16.125.37' identified by 'ssafy';
FLUSH PRIVILEGES;
```

## 5. mysql work brench 를 이용한 접속 확인하기 

![image](https://user-images.githubusercontent.com/46010705/58637396-5f785780-832d-11e9-8e8a-065e7e7f31ff.png)  





## 이슈 

1. 리눅스 mysql에는 한글이 들어가지 않아서 utf8로 테이블을 바꿔주는 것을 해줘야 했다.
2. 리눅스에 mysql을 설치했더니 root 권한을 인식하지 못한다. 권한을 다시 줘야했음 .  
```
grant all on *.* to 'root'@'3.16.125.37' identified by 'ssafy';
FLUSH PRIVILEGES;
```  
3. 비밀번호를 재수정해주는 코드 ( 필요하진 않은듯)
```
update mysql.user set plugin = 'mysql_native_password' where User='root';
```
4. mysql에서 table이 외래키를 가지고 있을 경우 에러가 발생했는데 해결하지 못함.





#### tomcat8/webapps 에있는 war 파일 삭제하기 
```
rm ~~.war
```
