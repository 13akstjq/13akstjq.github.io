---
layout: post
title:  "[AWS] AWS 시작하기"
date:   2019-05-27 22:28:59
author: 한만섭
categories: aws
tags:	aws
---

* TOC
{:toc}


## 아마존 웹 서비스 

아마존에서 전세계 곳곳에 컴퓨터를 설치해서 클라우드 시스템을 구추갷서 사용자들의 프로젝트를 서버에 대신 올려주는 역할을 해주는 서비스..







cloudping.info 에서 클라우드를 연결할때 걸리는 시간을 알 수 있음.


## Region

aws에서는 Region이라는 내용이 중요하다.
region과 region은 서로 데이터를 주고 받을 수 없다. (ex. 도쿄와 캐나다는 연결할 수 없음. )

## EC2

EC2(Elastic Compute Cloud)는 독립된 컴퓨터를 임대해주는 서비스 

인스턴스 = 컴퓨터 하나 

인스턴스를 삭제할 때 목록에서 사라지는데는 2시간정도 소요된다고 하니 기다리자.


## 가상머신 Linux 만들기

vCPUs = 가상 운영체제의 갯수 

Memory = 가상 운영체제의 메모리 

리눅스 형태로 만들경우 SSH를 허용해주어야 한다. 

윈도우 형태로 만들경우 RDP를 허용해주어야 한다.

사용자가 접속할 수 있게 하려면 HTTP를 사용해야한다. 

## EC2

`aws EC2 가격`으로 검색하면 페이지 나옴.


## ubunto

`ubunto`에 접속하기 위해선 ip , id , pw가 필요하다. 
`ubunto`로 접속할 때 id 는 ubunto 지만, 다른상태로 접속할 경우 id는 ec2-user이다.


## xshell 

sudo su root 를 해야 코드마다 앞에 sudo를 붙히지 않아도 된다. 

cd는 파일 이전경로로 가는 것 같다. 

파일을 설치할 수 없다고 할 때는 업데이트 해주고 하기 

`apt-get update`

`sudo su root`

`apt-get install openjdk-8-jre`

`apt-get install openjdk-8-jdk`

https://minimi22.tistory.com/4






