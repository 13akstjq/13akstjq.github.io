---
layout: post
title:  "[인스타클론코딩] [FrontEnd].10 - Add Comment"
date:   2019-07-13-16:57:00
author: 한만섭
categories: clonecoding
tags: react addComment
---

> ### 정리할 내용 

AutoSizeTextArea에 댓글을 입력했을 때 로딩화면 후에 곧바로 댓글에 추가가 되는 기능을 구현하려고 합니다.  
* addCommentMutation만들기  
* AutoSizeTextArea에서 Enter키 event 감지  
* commentInput 값 ""로셋팅  
* 추가되는 댓글 배열을 useState로 만들기   
* addCommentMutation이 response가 오면 추가 댓글을 넘겨주기  
* addCommentMutation을 호출하면 loading 보여주기  

***
> ### addCommentMutation만들기  

댓글을 달기 위해서는 유저 이름과 댓글 내용, 게시물 id가 필요로 합니다.  
`PostQueries.js`  
```javascript
export const ADD_COMMENT = gql`
  mutation addComment($postId: String!, $text: String!) {
    addComment(postId: $postId, text: $text) {
      id
      text
      user {
        id
        username
      }
    }
  }
`;
```

`PostContainer.js`  
```javascript
  const addCommentMutation = useMutation(ADD_COMMENT, {
    variables: { postId: id, text: commentInput.value }
  });
```

*** 

> ### AutoSizeTextArea에서 Enter키 event 감지 

누른 키를 알기 위해서는 **onKeyDown, onKeyUp, onKeyPress**를 사용할 수 있습니다. event 발생의 keyCode속성을 사용하면 다른 곳에선 사용할 수 있었는데, 
AutoSizeTextArea에서 사용하면 동작이 잘 되지 않았습니다. 그래서 **event.which === 13**일경우로 작성했습니다. (enter의 keycode === 13)   
```javascript
const onKeyPress = async e => {
    const { which } = e;

    if (which === 13) {
      
    }
  };
```

***  

> ### commentInput 값 ""로셋팅 

Hook을 이용해 UseInput을 사용하고 있었기 때문에 input의 value를 set하기 위해서는 useInput을 수정해야합니다. 아래와 같이 **setValue**도 return 해주면 됩니다.  
`useInput.js`  
```javascript
import { useState } from "react";

export default (defaultValue, type = "text") => {
  const [value, setValue] = useState(defaultValue);

  const onChange = e => {
    const {
      target: { value }
    } = e;
    setValue(value);
  };
  return { value, onChange, type, setValue };
};
```

하지만 위와 같이 작성을 하면 수정해줘야 할 부분이 있습니다. 기존에는 UseInput을 통째로 넣는 **{...commentInput}**으로 했다면 SetValue는 사용할 수 없기 때문에 
번거롭더라도 useInput의 value와 onChange만 넘겨주어야 합니다.  
`PostPresenter.js`  
```javascript
 <AutoSizeTextArea
          onKeyPress={onKeyPress}
          placeholder={"댓글 달기..."}
          value={commentInput.value}
          onChange={commentInput.onChange}
        />
```

엔터 이벤트가 발생했을 경우 제출되는 것을 막아주는 **e.preventDefault()**를 사용하고 **commentInput.setValue("")**로 값을 초기화 해줍니다.  
```javascript
 const onKeyPress = async e => {
    const { which } = e;

    if (which === 13) {
      e.preventDefault();
      commentInput.setValue("");
    }
  };
```


***

> ### 추가되는 댓글 배열을 useState로 만들기  

처음에 받아오는 댓글 말고 추가로 달린 댓글만 따로 그려주어야 하기 때문에 PostContainer에서 selfComments라는 UseState를 만들어줍니다.  

```javascript
  const [selfComments, setSelfComments] = useState([]);
```

엔터기가 발생했을 때 addCommentMutation을 발생시켜줍니다.  **async await**로 데이터를 받아오기 전까지 동작하지 못하도록 해줍니다. Mutation의 
기본 데이터 return 형식은 `{data : {~~~}}`인 형식인 것에 주의해야 합니다. addComment를 새로운 selfComments에 추가로 넣어주어야합니다. 
넣어주는 방법으로 **spread operator**를 사용했습니다. 기존의 **selfComments를 넣고 addComment를 추가하겠다는 의미입니다.  

```javascript
  const onKeyPress = async e => {
    const { which } = e;

    if (which === 13) {
      e.preventDefault();
      const {
        data: { addComment }
      } = await addCommentMutation();
      setSelfComments([...selfComments, addComment]);
      commentInput.setValue("");
    }
  };

```

***

> ### addCommentMutation을 호출하면 loading 보여주기  

이제 데이터를 받아오는 것은 성공했기 때문에 사용자 경험을 위해서 loading 을 구현해주어야 합니다. 제가 선택한 방법은 enter이벤트가 발생할때 만들어놓은
**loading**state를 true로 만들어주고 **response**가 왔을 경우 **loading**을 false로 해주는 방법을 썼습니다. loading State가 변하게 되면 UseEffect를
이용하여 loading을 falsef로 만들어주는 방식입니다.  

```javascript
const [loading, setLoading] = useState(false);

  useEffect(() => {
    setLoading(false);
  }, [selfComments]);

 const onKeyPress = async e => {
    const { which } = e;

    if (which === 13) {
      e.preventDefault();
      setLoading(true);
      const {
        data: { addComment }
      } = await addCommentMutation();
      setSelfComments([...selfComments, addComment]);
      commentInput.setValue("");
    }
  };
```

초기 loading=false -> enter발생 loading true -> comment response 시 selfComments 변경 -> selfComments와 연결시켜놓은 UseEffect발생 -> loading = false  

PostPresenter에서 Loading이 true 일경우 Loader컴포넌트를 보여주는 방식  


