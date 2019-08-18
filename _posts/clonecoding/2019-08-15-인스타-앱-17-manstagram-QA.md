```
layout: post
title: "[인스타클론코딩] [App].17 - manstagram QA "
date: 2019-08-13-18:39:00
author: 한만섭
categories: clonecoding
tags: react instagram netlify
```



[TOC]



## To Do

### front-end

- [x] Auth페이지 크기 조정 
- [x] Explorer 페이지 구성
- [x] 검색시 grid 적용 
- [x] 사진 비율 조정 
- [x] 로그아웃 

***



### App

- [ ] 사진 여러장 업로드 기능 

- [ ] 프로필 사진 추가 기능 

- [x] 게시물 추가시 Feed 밀림 현상 해결 

- [x] 프로필 페이지 당겨서 새로고침

- [x] 로그아웃

- [ ] 팔로워 팔로잉 보기 

- [ ] 메세지 기능 

- [ ] 구독 알림기능 

- [x] 유저 검색

- [x] 사진 선택시 이미지 가로보임

- [x] 댓글 더보기 

  

  

  

  

  

### - 사진 선택시 이미지 가로보임 

```js
 <ScrollView
            contentContainerStyle={{
              flexDirection: "row",
              flexWrap: "wrap"
              // width: constants.width
            }}
          >
```

***



### - 게시물 추가시 Feed 밀림 현상 해결 

***



### - 프로필 페이지 당겨서 새로고침

refreshcontrol

```js
 const [editUser] = useMutation(EDIT_USER, {
    variables: {
      avatar: url
    }
  });
```

***



### - useQuery 호출시 첫 undefined 무시하기

```js
const furstUpdat = useRef(true);
```

```js
 useEffect(() => {
    if (firstUpdate.current) {
      firstUpdate.current = false;
      return;
    }
    console.log("Effect", url);
    editUser();
  }, [url]);
```



useEffect의 첫번째 render를 막기위해서 

useRef를 사용해서 처음에 true이면 false로 바꿔줌. 

***

