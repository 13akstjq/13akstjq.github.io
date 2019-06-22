---
layout: post
title:  "[GraphQL] GraphQL 개발 이슈  "
date:   2019-06-19-14:10:00
author: 한만섭
categories: graphql
tags: GraphQL
---

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
