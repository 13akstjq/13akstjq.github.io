---
layout: post
title:  "[상태관리] Redux"
date:   2019-10-10-22:28:00
author: 한만섭
categories: blockcar
tags: blockcar Redux TypeScript ReactHooks actions types reducer store
---



* TOC
{:toc}




![image](https://user-images.githubusercontent.com/46010705/66573699-10c9ac80-ebae-11e9-95fd-e3f32c8421f6.png)

***





### Q. ReactHooks + TypeScript+ Redux 를 어떻게 사용했나요? 



- Reducer 만들기 

  - `reducer/auth/authTypes.tsx`

  ```js
  // action의 type의 type
  export const TOGGLE_SIGNUP_POPUP = "TOGGLE_SIGNUP_POPUP";
  export const TOGGLE_SIGNIN_POPUP = "TOGGLE_SIGNIN_POPUP";
  export const LOGIN = "LOGIN";
  export const LOGOUT = "LOGOUT";
  
  // authReducer의 initialState type
  export interface IAuthState {
    isSignUpPopUpShow: boolean;
    isSignInPopUpShow: boolean;
    loggedInUser: user;
  }
  
  // actionCreator 의 type
  interface showSignUpPopUpAction {
    type: typeof TOGGLE_SIGNUP_POPUP;
    isSignUpPopUpShow: boolean;
  }
  
  interface showSignUpPopInAction {
    type: typeof TOGGLE_SIGNIN_POPUP;
    isSignInPopUpShow: boolean;
  }
  
  export interface user {
    id: string;
    seq: number;
    type: string;
  }
  
  interface logInAction {
    type: typeof LOGIN;
    user: user;
  }
  
  interface logOutAction {
    type: typeof LOGOUT;
  }
  
  export type AuthActionType =
    | showSignUpPopUpAction
    | showSignUpPopInAction
    | logOutAction
    | logInAction;
  
  ```

  - 로그인, 회원가입 화면을 보여주거나 숨기는 액션의 이름, Type 정의 

  - AuthReducer에서 사용될 State의 Type 정의 

  - Action은 각각 export type 하는 것이 아니라 하나의 `AuthActionType`이라는 Type으로 사용했습니다.  

    ```js
    export type AuthActionType =
      | showSignUpPopUpAction
      | showSignUpPopInAction
      | logOutAction
      | logInAction;
    ```

  - `reducer/auth/authActions.tsx`

    ```js
    import {
      AuthActionType,
      TOGGLE_SIGNUP_POPUP,
      TOGGLE_SIGNIN_POPUP,
      LOGIN,
      LOGOUT,
      user
    } from "./authTypes";
    
    // actionCreator 정의
    export const toggleSignUpPopUp = (
      isSignUpPopUpShow: boolean
    ): AuthActionType => {
      return {
        type: TOGGLE_SIGNUP_POPUP,
        isSignUpPopUpShow
      };
    };
    
    export const toggleSignInPopUp = (
      isSignInPopUpShow: boolean
    ): AuthActionType => {
      return {
        type: TOGGLE_SIGNIN_POPUP,
        isSignInPopUpShow
      };
    };
    
    export const logInAction = (user: user) => {
      return {
        type: LOGIN,
        user
      };
    };
    
    export const logOutAction = () => {
      return {
        type: LOGOUT
      };
    };
    
    ```

    - `Actions.tsx`파일에서는 Action을 return 해주는 ActionCreator를 정의하는 곳입니다.  

  - `reducer/auth/authReducer.tsx`

    ```js
    import {
      IAuthState,
      AuthActionType,
      TOGGLE_SIGNUP_POPUP,
      TOGGLE_SIGNIN_POPUP,
      LOGIN,
      LOGOUT
    } from "./authTypes";
    
    const initialState: IAuthState = {
      isSignUpPopUpShow: false,
      isSignInPopUpShow: false,
      loggedInUser: JSON.parse(sessionStorage.getItem("LoggedInUser"))
    };
    
    const authReducer = (
      state = initialState,
      action: AuthActionType
    ): IAuthState => {
      switch (action.type) {
        case TOGGLE_SIGNUP_POPUP:
          return {
            ...state,
            isSignUpPopUpShow: !state.isSignUpPopUpShow
          };
        case TOGGLE_SIGNIN_POPUP:
          return {
            ...state,
            isSignInPopUpShow: !state.isSignInPopUpShow
          };
        case LOGIN:
          sessionStorage.setItem("LoggedInUser", JSON.stringify(action.user));
          return {
            ...state,
            loggedInUser: action.user
          };
        case LOGOUT:
          sessionStorage.removeItem("LoggedInUser");
          return {
            ...state,
            loggedInUser: null
          };
        default:
          return state;
      }
    };
    
    export default authReducer;
    
    ```

    - `Reducer.tsx`파일에서는 Reducer에 사용될 초기 State인 initialState를 정의했고, Dispatch에 의한 Action이 들어왔을 때 어떤 동작을 할 것인지 정의하는 Reducer를 만들었습니다.  

  - `reducer/index.tsx`

    ```js
    import HeaderReducer from "./header/headerReducer";
    import SearchPageReducer from "./searchPage/searchPageReducer";
    import AuthReducer from "./auth/authReducer";
    import MyPageReducer from "./myPage/myPageReducer";
    import { IMyPageState } from "./myPage/myPageTypes";
    import { combineReducers } from "redux";
    import { IHeaderState } from "./header/headerTypes";
    import { ISearchPageState } from "./searchPage/searchPageTypes";
    import { IAuthState } from "./auth/authTypes";
    export interface rootState {
      HeaderReducer: IHeaderState;
      SearchPageReducer: ISearchPageState;
      AuthReducer: IAuthState;
      MyPageReducer: IMyPageState;
    }
    
    const rootReducer = combineReducers({
      HeaderReducer,
      SearchPageReducer,
      AuthReducer,
      MyPageReducer
    });
    export default rootReducer;
    
    ```

    - `index.tsx`에서는 프로젝트에 사용하는 모든 Reducer를 import 해서 redux의 combineReducers를 사용해 rootReducer로 묶어줍니다.  
    - rootReducer도 type을 정해줘야 하는데 rootReducer에 사용되는 모든 reducer의 initialState를 사용하면 됩니다.  

***



- reducer 사용하기 

   　  

  - useSelector

    - reducer를 만들어보았으니 이제 ReactHooks를 사용해서 store의 값에 접근해보도록 하겠습니다.  로그인을 하는 `SignIn.tsx`컴포넌트에서 사용해보겠습니다.  

      `src/Components/Auth/SignIn.tsx`

      코드 이해를 위해 많은 부분을 생략한 점 양해 부탁드립니다.    

      ```js
      import React from "react";
      import {  useSelector } from "react-redux";
      import { rootState } from "../../../reducer";
      
      (...중략...)
      
      const SignIn = () => {
          
        const { isSignInPopUpShow, isSignUpPopUpShow } = useSelector(
          (state: rootState) => state.AuthReducer
        );
      
      
        (...중략...)
      };
      
      export default SignIn;
      
      ```

      위 코드를 보면 `useSelector`를 사용한 것을 확인하실 수 있습니다. `useSelector`는 Reducer의 Store에 접근해서 state값을 Select해주는 기능을 합니다. 즉, State의 값을 가져와 사용할 수 있게 해줍니다.  

      useSelect는 rootReducer의 State를 paramater로 가지는 함수를 넣어줘야합니다.  

      ```js
        const { isSignInPopUpShow, isSignUpPopUpShow } = useSelector(
          (state: rootState) => state.AuthReducer
        );
      ```

      위 코드처럼 작성하면 state의 AuthReducer를 return 하게 되고 AuthReducer에 있는 isSignInPopUpShow, isSignUpPopUpShow를 Es6의 문법인 distructuring을 통해서 const 상수에 담았습니다.  

      이렇게 하면 useSelector를 사용해서 Reducer의 State에 접근할 수 있습니다. 

      　  

  - useDispatch사용하기 

    - useSelector가 Reducer의 State를 읽는 기능이라면 useDispatch는 State의 값을 변경시키는 mutation이라고 생각하시면 됩니다.  

      ```js
      import React from "react";
      import styled from "styled-components";
      import Theme from "../../Styles/Theme";
      import useInput from "../Hooks/useInput";
      import Button from "../Commons/Button";
      import { useDispatch, useSelector } from "react-redux";
      import {
        toggleSignInPopUp,
        toggleSignUpPopUp,
        logInAction
      } from "../../../reducer/auth/authActions";
      import { rootState } from "../../../reducer";
      import { signIn } from "../../../services/user/signIn";
      
      (...styled-component 생략)
      
      const SignIn = () => {
        const idInput = useInput("");
        const pwInput = useInput("");
        const dispatch = useDispatch();
        const { isSignInPopUpShow, isSignUpPopUpShow } = useSelector(
          (state: rootState) => state.AuthReducer
        );
        const siginClick = async (e: React.FormEvent) => {
          e.preventDefault();
          if (idInput.value === "") {
            alert("아이디를 입력해주세요");
          } else if (pwInput.value === "") {
            alert("비밀번호를 입력해주세요");
          } else {
            const res = await signIn(idInput.value, pwInput.value);
            // 로그인 성공 
            if (res) {
              const loggedInUser = {
                id: res.id,
                seq: res.seq,
                type: res.type
              };
              dispatch(logInAction(loggedInUser));
              dispatch(toggleSignInPopUp(false));
            } else {
              // 로그인 실패 
              alert("아이디 혹은 패스워드를 다시 확인하세요.");
            }
          }
        };
      
        (...중략...)
         
      };
      
      export default SignIn;
      
      ```

      위 코드에서 봐야할 부분만 따로 확인해보겠습니다.  

      ```js
      const SignIn = () => {
      
       (...중략...)
      
      	const dispatch = useDispatch();
        
        const siginClick = async (e: React.FormEvent) => {
        (...중략...)  
              dispatch(logInAction(loggedInUser));
              dispatch(toggleSignInPopUp(false));
        };
      
        (...중략...)
         
      };
      
      ```

      useDispatch()를 통해서 간단히 dispatch를 만들 수 있습니다.   

      dispatch의 파라메터는 Reducer를 만들때 작성했던 ActionCreator를 넣어주면 됩니다. 위의 상황은 로그인에 성공했다면 `loginAction()`을 호출하는 것입니다.  

    

  

***



### Q. ReactHooks + TypeScript+ Redux를 사용하며 발생한 이슈

- rootState의 type은 도대체 뭐죠?

  ```js
    const { isSignInPopUpShow, isSignUpPopUpShow } = useSelector(
      state => state.AuthReducer
    );
  ```

  위와 같이 코드를 작성하게 되면 state의 Type이 없다면서 에러를 발생하게 됩니다. 저는 RootReducer의 State의 Type을 어떻게 정해줘야할지 몰라서 고민을 많이 했습니다.  

  해결하기 위해서 state를 console로 확인해보았고, state는 제가 combine했던 Reducer들이 들어있었습니다.   

  그래서 각 Reducer들의 State를 담고 있는 rootState를 만들어서 사용했더니 이슈가 해결되었습니다.  

  ```js
  export interface rootState {
    HeaderReducer: IHeaderState;
    SearchPageReducer: ISearchPageState;
    AuthReducer: IAuthState;
    MyPageReducer: IMyPageState;
  }
  
  ```

  



***



　 

### Q. ReactHooks + TypeScript+ Redux를 개발하며 느낀 점 

- 세가지 기술 스택에 익숙하지 않은채로 섞어서 개발을 하다보니 이곳 저곳에서 에러가 발생하는데 정확한 원인을 알기 어려웠습니다. 

- 초기에는 '도대체 typescript로 redux를 작성하면 뭐가 좋은거지? type정해주는데 시간만 엄청 걸리잖아!!! '라고 생각했습니다. 하지만, useSelector를 사용한 이후에 바로 '아 typescript가 없었다면 큰일났었겠다....'라는 생각을 했습니다.  

- useSelector를 사용하면 rootState에 접근하게 되는데 이 때 모든 Reducer에 대한 type을 정해놨기 때문에 매우매우매우 강력한 **자동완성**을 제공해줍니다. 사실 저는 자동완성 하나만 보고도 사용해야할 명분이 있다고 생각할 정도로 편했습니다.  

- 또한 useContext를 사용할 때 보다 상태 관리하는 폴더의 구조가 이해하기 쉬웠던 것 같습니다.  

- 개인적으로 Context는 코드가 짧아서 편할줄 알았지만 사용할 때 안정적인것은 위 조합을 사용할 때 였던 것 같습니다.  

  