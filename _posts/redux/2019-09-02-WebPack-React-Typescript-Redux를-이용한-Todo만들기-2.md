---
layout: post
title: "[redux] WebPack React Typescript Redux를 이용한 Todo만들기 -2 "
date: 2019-08-27 22:05:00
author: 한만섭
categories: redux
tags: redux WebPack react Typescript redux
---

- TOC
  
  {:toc}

## 1. reducer types 정의하기

todo reducer를 만들 경우, 파일 경로는 `./store/todo/`에 만들어주면 됩니다.

reducer의 type을 정의 하기위해 types 파일을 만들어줍니다. 여기에는 reducer의 *initialState*의 type

과 *action*의 type을 정의합니다.

`./store/todo/types.tsx`

```js
/*
type을 정의하고 todoAction의 type을 정의하는 곳  
*/

export const ADD_TODO = "ADD_TODO";
export const TOGGLE_TODO = "TOGGLE_TODO";
export const DELETE_TODO = "DELETE_TODO";

export interface Todo {
  index: number;
  text: string;
  isCompleted: boolean;
}

export interface TodoState {
  todos: Todo[];
}

interface AddTodoAction {
  type: typeof ADD_TODO;
  text: string;
}

interface ToggleTodoAction {
  type: typeof TOGGLE_TODO;
  index: number;
}

interface DeleteTodoAction {
  type: typeof DELETE_TODO;
  index: number;
}

export type TodoActionTypes =
  | AddTodoAction
  | ToggleTodoAction
  | DeleteTodoAction;
```

## 2. Action Creator, Action 만들기

`./store/todo/action.tsx`

```js
import { ADD_TODO, TOGGLE_TODO, DELETE_TODO } from "./types";

export const AddTodoAction = (text: string) => {
  return {
    tyep: ADD_TODO,
    text
  };
};

export const ToggleTodoAction = (index: number) => {
  return {
    type: TOGGLE_TODO,
    index
  };
};

export const DeleteTodoAction = (index: number) => {
  return {
    type: DELETE_TODO,
    index
  };
};
```

## 3. reducer 만들기

reducer를 만들 때는 type들을 지정해줄 것이 많기 때문에 주의해야합니다.

### 3-1. reducer를 만들 때 주의할 점

1.  각 **case**마다 return 을 해주는 데 그 값은 state를 return 해주는 것 입니다.
2.  **map**으로 코드를 작성할 떄는 매번 **return**을 해줘야 합니다.

`./store/todo/reducer.tsx`

```js
import {
  TodoState,
  TodoActionTypes,
  ADD_TODO,
  TOGGLE_TODO,
  DELETE_TODO
} from "./types";

const initialState: TodoState = {
  todos: [{ index: 1, text: "할일1", isCompleted: false }]
};

const todoReducer = (state = initialState, action: TodoActionTypes) => {
  switch (action.type) {
    case ADD_TODO: {
      console.log("ADD_TODO");
      return {
        todos: [
          ...state.todos,
          {
            index: state.todos.length + 1,
            text: action.text,
            isCompleted: false
          }
        ]
      };
    }
    case TOGGLE_TODO: {
      console.log("TOGGLE_TODO");
      return {
        todos: state.todos.map(todo => {
          if (todo.index === action.index) {
            todo.isCompleted = !todo.isCompleted;
          }
          return todo;
        })
      };
    }
    case DELETE_TODO: {
      console.log("DELETE_TODO");
      return {
        todos: state.todos.filter(todo => todo.index !== action.index)
      };
    }
    default:
      return state;
  }
};

export default todoReducer;
```



<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="4307878116"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>





## 4. Input, Form 만들기

`input`의 onChange를 사용하기 위해서는 `React.ChangeEvent<HTMLInputElement>`를 사용해야합니다.

`form`의 제출 이벤트는 `React.FormEvent`를 사용해야합니다.

`./Components/TodoInput.tsx`

```js
import React, { FunctionComponent, useState, InputHTMLAttributes } from "react";
import { useDispatch } from "react-redux";
import styled from "styled-components";
import { ADD_TODO } from "../store/todo/types";

const Input = styled.input``;

const TodoInput: FunctionComponent = () => {
  const [value, setValue] = useState("");
  const dispatch = useDispatch();
  const onChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    setValue(e.target.value);
  };

  const onSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    setValue("");
    dispatch({ type: ADD_TODO, text: value });
  };

  return (
    <form onSubmit={onSubmit}>
      <Input value={value} onChange={onChange}></Input>
    </form>
  );
};

export default TodoInput;
```

## 5. List 만들기

여기서 잘못 작성한 것은 **_ActionCreator_**를 사용하지 않고 *inline*으로 바로 넣은 것에서 문제가 있었습니다.

`Components/TodList.tsx`

```js
import React, { FunctionComponent } from "react";
import { useSelector, useDispatch } from "react-redux";
import styled from "styled-components";
import { TodoState, TOGGLE_TODO, DELETE_TODO } from "../store/todo/types";

const Wrapper = styled.div`
  min-height: 500px;
  border: 1px solid black;
`;

const Todo = styled.div``;

const Text =
  styled.span <
  { isCompleted: boolean } >
  `
  color: ${props => (props.isCompleted ? "#999" : "black")};
`;
const TodoList: FunctionComponent = () => {
  const todos = useSelector((state: TodoState) => state.todos);
  const dispatch = useDispatch();
  console.log(todos);
  return (
    <Wrapper>
      {todos.map(todo => (
        <Todo key={todo.index}>
          <Text isCompleted={todo.isCompleted}>{todo.text}</Text>
          <button
            onClick={() => dispatch({ type: TOGGLE_TODO, index: todo.index })}
          >
            완료
          </button> <button onClick={() => dispatch({ type: DELETE_TODO, index: todo.index })}>삭제</button>
        </Todo>
      ))}
    </Wrapper>
  );
};

export default TodoList;
```
