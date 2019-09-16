---
layout: post
title: "[It's me][backend] datamodel 설계하기 "
date: 2019-09-14-16:47:00
author: 한만섭
categories: itsme-backend
tags: itsme jobSlider component
---

- TOC
  
  {:toc}







### 1. datamodel 설계 

아래와 같이 데이터 모델을 개략적으로 설계를 해보았습니다. 

\### itsme table



\## User



\- name : string!

\- age : number!

\- job : String! (ex.프론트엔드)

\- careers : [Career]!

\- skills : [string]!

\- possibleRegions : [string]! (판교,강남)

\- minSalary : number

\- maxSalary : number

\- projects : [Project]

\- school: string!

\- major : string!

\- profileImage : Image

\- liveRegion : string!

\- phoneNumber : string!

\- grade : Double!

\- tmi : [string]! (선택할 수 있게 함.)

\- selfInterview : [QnA]!

\- viewCount : number!

\- likeCount : number!

\- proposedInterview : [propose]!



\## Project



\- title : string! (이츠미)

\- desc : string! (고용사이트입니다. )

\- duration : Duration!

\- skills : [string]!

\- myTasks : [string]! (어느어느기술을 이용해서 로그인 부분을 구현했음.)

\- mainImage : Image!

\- subImages : [Image]!

\- isNowProject : boolean!



\## Career



\- duration : Duration!

\- title : string! ( 멀티캠퍼스 )

\- desc : string! ( 무슨무슨 일했음 )

\- isNowCareer : boolean!



\## Duration



\- start : DateTime!

\- end : DateTime



\## Image



\- url : string!

\- project : Project!



\## QnA



\- question : string!

\- answer : string!



\## Company



\- title : string!

\- desc : string!

\- mainImage : Image!

\- subImages : [Image]!

\- address : string!



\## Propose



\- from : Company!

\- to : User!

\- state : string!

\- createdAt : DateTime!



위와 같이 설계한 데이터 모델을 `datamodel.prisma`에 작성하도록하겠습니다.  



`datamodel.prisma`

```js
type User {
  id: ID! @id
  name: String!
  age : Float!
  job : String!
  careers : [Career]!
  skills : [Skill!]!
  possibleRegion : [Region!]!
  minSalary : Float
  maxSalary : Float
  projects : [Project]
  school : String!
  major : String!
  profileImage : Image
  liveRegion : String!
  phoneNumber : String!
  grade : Float!
  tmis : [TMI!]!
  selfInterview : [QnA]!
  viewCount : Float!
  likeCount : Float!
  proposedInterView : [Propose]!
}

type TMI {
  id : ID! @id 
  title : String!
}

type Skill {
  id : ID! @id 
  title : String!
}

type Region {
  id : ID! @id 
  name : String!
}

type Project {
  id : ID! @id
  title : String!
}

type Career {
  id: ID! @id
  duration : Duration!
}

type Duration {
  id: ID! @id
  start : DateTime!
  end : DateTime!
}

type Image {
  id: ID! @id
  url : String!
  project : Project!  
}

type QnA {
  id: ID! @id
  question : String!
  answer : String!
}

type Company {
  id: ID! @id
  title : String!
  desc : String!
  mainImage : Image! 
  subImage : [Image!]!
  address : String!
}

type Propose {
  id: ID! @id
  from : Company!
  to : User!
  state : String!
  createdAt : DateTime! @createdAt
}
```



### 2. ScalarType 이슈

![1568560201356](../../../../assets/image/1568560201356.png)

데이터 모델을 작성하던 도중 **ScalarType**이슈가 나왔는데, 해당 이슈는 prisma에서 사용하는 dataType을 사용하지 않았을 때 발생하는 이슈였습니다.   



[참고 사이트](https://github.com/prisma/prisma/issues/1753)에 나와 있는 데이터 type을 사용하면 될 것 같습니다.  

![1568560047649](../../../../assets/image/1568560047649.png)



그리고 type을 작성할 때 `[String]`은 작성이 되지 않았습니다. 그래서 지역, 스킬 같은 속성들도 하나의 type으로 만들어서 아래와 같이 작성해야 합니다. 

```jsㅐㅜ
type User {
	skills : [Skill!]!
}

type Skill {
  id : ID! @id 
  title : String!
}
```





### Link 이슈 

![1568563450577](../../../../assets/image/1568563450577.png)

```
type Project {
  id : ID! @id
  title : String!
  desc : String!
  duration : Duration!
  skills : [Skill]!
  myTasks : [Task]!
  mainImage : Image! @relation(link: INLINE)
  isNowProject : Boolean!
}
```

이유는 아직 잘 모르겠으나 mainImage 를 공유해서 그런 건지 link로 연결을 해줘야했습니다.  

정리가 필요할 것 같습니다.  

