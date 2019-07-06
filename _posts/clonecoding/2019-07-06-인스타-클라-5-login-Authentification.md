---
layout: post
title:  "[인스타클론코딩] [FrontEnd].5 - login Authentification 완성하기 "
date:   2019-07-06-14:47:00
author: 한만섭
categories: clonecoding
tags: react instagram
---


> ## To Do 

- [X] CreateAccount 
- [] ConfirmSecret
- [] @client Mutation 
　  

***

　  

> ## 1. CreateAccount 

계정을 만들기 위해선 우선 Query를 만들어야합니다. 저는 Qeury를 **Auth/AuthQueries.js**에 작성하고 있습니다.  

> ### Auth/AuthQueries.js
```
(...중략...)

export const CREATE_ACCOUNT = gql`
  mutation createAccount(
    $email: String!
    $username: String!
    $firstName: String!
    $lastName: String!
  ) {
    createAccount(
      email: $email
      username: $username
      firstName: $firstName
      lastName: $lastName
    )
  }
`;

(...중략...)
```

Query를 만들었다면 AuthContainer에서 사용해야 합니다. 제가 만든 것은 Mutation이기 때문에 mutation을 사용하는 방법을 알아야 합니다. [react-apollo-hooks](https://github.com/trojanowski/react-apollo-hooks) 
에서 사용 방법을 확인할 수 있습니다. 간단하게 **useMutation**을 사용하고 **variables**로 인자를 전달하는 방식입니다.  

> ### Auth/AuthContainer.js  
```
(...중략...)

  const createAccountMutation = useMutation(CREATE_ACCOUNT, {
    variables: {
      email: email.value,
      username: username.value,
      firstName: firstName.value,
      lastName: lastName.value
    }
  });
  
(...중략...)
```
전달 하는 variables를 주의해서 봐야할 것 같습니다. 회원가입을 할 경우 4개의 input에 입력한 value를 사용해서 회원가입을 하게 되는데, 그 때 
input의 value를 가져오기 위해 **useStat**를 이용해 **useInput**이라는 Hook을 사용했습니다. 여기서 useInput에 대해 간단하게 정리만 하도록 하겠습니다.  

> ### Hooks/useInput.js  
```
import { useState } from "react";

export default (defaultValue, type = "text") => {
  const [value, setValue] = useState(defaultValue);

  const onChange = e => {
    const {
      target: { value }
    } = e;
    setValue(value);
  };
  return { value, onChange, type };
};

```
`useinput`에는 input 에 사용될 **defaultValue**와 input의 **type**을 입력받습니다. 또한 input에 사용자가 입력하는 것을 알기 위해 **onChange**라는 
메소드도 만들어 놓습니다.  

```
const onChange = e => {
    const {
      target: { value }
    } = e;
    setValue(value);
  };
```
이 부분을 보게 되면 **e**라는 이벤트를 받게 됩니다. e는 아래와 같은 구조로 되어 있습니다.  
```
e { 
  target : {
    value : {???}
    }
  }
```
여기서 value가 input의 value를 뜻하게 됩니다. 그렇기 때문에 e.target.value를 변경시켜주면 input의 value가 변경되게 되는 것입니다. 그래서 아래와 같은 함수를 작성하는 것이기도 합니다.  
```
 const {
      target: { value }
    } = e;
```

***

이제 useInput을 사용해서 회원가입 form 이 제출될 경우 prisma에 회원가입을 요청하고 error처리를 해야합니다. 아래 코드는 회원가입 버튼을 눌렀을 때 
발생하는 submit 부분입니다.  

> ### Auth/AuthContainer.js  
```
 const onSubmit = async e => {

(...중략...)

else if (action === "signUp") {
        console.log("singUp");
        if (
          email.value !== "" &&
          username.value !== "" &&
          firstName.value !== "" &&
          lastName.value !== ""
        ) {
          try {
            const { data: createAccount } = await createAccountMutation();
            if (createAccount) {
              toast.success("Success createAccount!! ");
              setTimeout(() => {
                setAction("logIn");
                email.value = "";
              }, 2000);
            }
          } catch (e) {
            toast.error(e.message);
          }
        }
      } 
      
(...중략...)
}
```

Input이 비어있지 않다면 위에서 만들었던 `createAccount()`를 실행시키게 됩니다. 여기서 가장 중요한점은 `await로 prisma의 응답을 기다리는 것`입니다. 
onSubmit함수에 async await를 사용하지 않을 경우 **response**가 오기전에 다음 코드를 진행하기 때문에 정상적인 진행이 되지 않습니다.  

await에 실패했을 경우 **catch**를 동작하고 await에서 에러가 발생하지 않았다면 toast로 성공 메세지를 띄어주고 `setAction()`으로 보여주는 화면을 
전환 해줍니다.  

한가지 의문이 있다면 이렇게 Action을 변경하는 방식으로 보여지는 page를 변경시켜준다면 새로고침을 할경우 데이터가 날라가게 되는데 주소에 `/signUp`을 
사용하는 방식이 좋은지 `setAction('signUp')이 좋은지는 추가적으로 공부해봐야 알 수 있을 것 같습니다.  
　  
   
***

　  
> ## ConfirmSecret
