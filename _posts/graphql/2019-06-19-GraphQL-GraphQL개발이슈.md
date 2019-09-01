---
layout: post
title:  "[GraphQL] GraphQL 개발 이슈  "
date:   2019-06-19-14:10:00
author: 한만섭
categories: graphql
tags: GraphQL
---

* TOC
{:toc}

> ### 1. query의 argument를 정하는 방법
![image](https://user-images.githubusercontent.com/46010705/59738578-b625e100-929c-11e9-88f4-e2ee76a3ddac.png)
```
{
	person(id:3){
    name
  }
}
```
위와 같이 person에 id라는 인자를 주고 싶다면 직접 어떤 argument인지 정해줘야 한다. 
　  

***

　  

> ### 2. resolver에서 query 선언하기 
	
  ```
  person : (_,{id}) => getById(id)
  ```
  위와 같이 첫번째 인자는 아직 사용하지 않기 떄문에 자세히 잘 모르기 때문에 `_`로 사용하고, 두번째에 인자를 적어넣는다. {id} == args.id


> ### 3. filter를 하면 배열 형태로 나오기 때문에 하나를 원하면 [0]해야함.
	
	```
	export const getById = (id) => {
	    const filteredmovies = movies.filter(movie => movie.id === id);
	    // console.log(filteredpeople );
	    return filteredmovies[0]; 
	}
	```


> ## 포트 4000번 중복 이슈 

> ### 시작하기전에
  server를 시작하려하는데 자꾸 에러가 발생해서 자꾸 내 코드를 의심했던 에러.... 내 지식에 대한 확신이 없는건 더 큰 화를 부른다는 좋은 예시. 
  개념을 잘 잡고 공부해야 실수도 적고 실수했을 때 어떤 부분일지 예상이 쉽게 되는 것 같다. 

　  
   
*** 

　  
아래와 같은 에러가 발생하면 포트를 죽였을 때 될 가능성이 높다!
```
> movie-graphql@1.0.0 start C:\Users\mshan\Desktop\react-workspace\movie-graphql
> nodemon --exec babel-node index.js

[nodemon] 1.19.1
[nodemon] to restart at any time, enter `rs`
[nodemon] watching: *.*
[nodemon] starting `babel-node index.js`
events.js:177
      throw er; // Unhandled 'error' event
      ^

Error: listen EADDRINUSE: address already in use :::4000
    at Server.setupListenHandle [as _listen2] (net.js:1226:14)
    at listenInCluster (net.js:1274:12)
    at Server.listen (net.js:1362:7)
    at C:\Users\mshan\Desktop\react-workspace\movie-graphql\node_modules\graphql-yoga\src\index.ts:388:22
    at new Promise (<anonymous>)
    at GraphQLServer.start (C:\Users\mshan\Desktop\react-workspace\movie-graphql\node_modules\graphql-yoga\src\index.ts:386:12)
    at Object.<anonymous> (C:/Users/mshan/Desktop/react-workspace/movie-graphql/index.js:8:8)
    at Module._compile (internal/modules/cjs/loader.js:774:30)
    at loader (C:\Users\mshan\Desktop\react-workspace\movie-graphql\node_modules\babel-register\lib\node.js:144:5)
    at Object.require.extensions.<computed> [as .js] (C:\Users\mshan\Desktop\react-workspace\movie-graphql\node_modules\babel-register\lib\node.js:154:7)
Emitted 'error' event at:
    at Server.emit (events.js:200:13)
    at Server.EventEmitter.emit (domain.js:471:20)
    at emitErrorNT (net.js:1253:8)
    at processTicksAndRejections (internal/process/task_queues.js:84:9) {
  code: 'EADDRINUSE',
  errno: 'EADDRINUSE',
  syscall: 'listen',
  address: '::',
  port: 4000
}
[nodemon] app crashed - waiting for file changes before starting...
```


> ### 포트 죽이는 방법

* 포트번호확인하기 
```
netstat -a -o
```

나온 결과에서 사용중이라는 포트번호에 속하는 PID값을 아래의 command에 입력하면 

```
taskkill /f /pid [PIDNUMBER]
```

# 포트 자식 죽여버리기 성공. 아주 정당한 죽임!
