---
layout: post
title:  "[인스타클론코딩] [FrontEnd].4 - presenterContainer & Query 만들기 "
date:   2019-07-05-17:47:00
author: 한만섭
categories: clonecoding
tags: react instagram
---

> ### 정리할 내용
- [X] Container & Presenter & index Queries로 나누기 
- [X] useMutation 사용하기 
- [X] 이메일이 무조건 보내졌던 이슈 해결 
- [X] {}-> [] 디버깅 
- [X] useState를 이용해서 useInput만들기 
- [X] toast box 사용해서 에러 표시하기 
- [X] useMutation 에서 update사용하기 
- [X] cretaeAccount return true/false로 바꾸기 


> ### Page를 Container, Presenter, Queries, index

Container은 **Query,Hook,method,props**를 빼서 **Presentre**에 전해주는 역할을 합니다.  
Presenter은 이름 그대로 보여지는 부분을 담당합니다.  
Queries는 서버에 보낼 Query를 정의하는 부분입니다.  
index는 Container를 import해서 export 시키는 곳입니다.  


> ### UseMutation 사용하기  

[github사이트](https://github.com/trojanowski/react-apollo-hooks)  

첫번째 인자로는 Query명을 사용하고, 다음에는 option을 넣는 곳입니다. `update`는 result를 얻을 수 있는 옵션이고, `variables`를 통해서 **arguments**를 넣을 수 있습니다.  

```
 const createAccount = useMutation(CREATE_ACCOUNT, {
    update: (_, result) => {
      console.log(result);
    },
    variables: {
      email: email.value,
      username: username.value,
      firstName: firstName.value,
      lastName: lastName.value
    }
  });
```

> ### 이메일이 무조건 보내졌던 이슈 

이메일이 계쏙 보내졌던 이슈는 해당 이메일을 갖고 있는 유저가 있는지 확인하기 전에 이메일을 보냈기 때문에 발생했습니다.  

> ### useState를 사용하기 

**[]**인데 **{}**쓰지않기
```
const [action, setAction] = useState("logIn");
```

> ### Toast box 사용하기  

[npm사이트](https://www.npmjs.com/package/react-toastify)  

* import
```
 import { ToastContainer, toast } from 'react-toastify';
 import 'react-toastify/dist/ReactToastify.css';
```

ToastContainer은 Tag로 사용합니다.  
```
<ToastContainer position={"bottom-left"} />
```

toast는 메소드로 사용합니다.  
```
toast(`you can logIn before signUp!!!`);
```

