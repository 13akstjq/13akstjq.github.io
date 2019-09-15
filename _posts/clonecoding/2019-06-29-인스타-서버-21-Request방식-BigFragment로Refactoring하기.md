---
layout: post
title: "[인스타클론코딩] [Server].21 - Request방식 Fragment로 Refactoring하기   "
date: 2019-06-29-02:33:00
author: 한만섭
categories: clonecoding
tags: instagram post fragment refactoring
---

### 정리할 내용

하나의 요청을하기 위해 많은 request를 사용해야하는 경우가 있습니다. 예를 들면 게시물 정보를 보기위해 user를 요청하고 file,post,comment를 요청하는
상황이 발생할 수 있습니다. 하지만 이런 상황에서 커다란 fragment하나를 사용한다면 코드를 좀더 간략하게 짤 수 있을 것 입니다.

---

### Fragment Refactoring

- refactoring 하기전의 코드

  아래와 같이 post, comments, files, user를 요청하는 여러줄의 코드가 있습니다. 이것을 하나의 fragment로 해결하려고 합니다.

  ```
  import { prisma } from '../../../../generated/prisma-client';
  import { COMMENT_FRAGMENT } from '../../../fragments';

  export default {
      Query: {
          seeFullPost: async (_, args, { request, isAuthenticated }) => {
              isAuthenticated(request);
              const { id } = args;
              //post , comments , likeCount
              const post = await prisma.post({ id });
              const comments = await prisma
                  .post({ id })
                  .comments()
                  .$fragment(COMMENT_FRAGMENT);
              const files = await prisma.post({ id }).files();
              const user = await prisma.post({ id }).user();
              return {
                  post,
                  comments,
                  files,
                  user
              };
          }
      }
  };
  ```

- After Refactoring

  refactoring 방법으로는 user,file,comment와 같은 fragment를 먼저 만들고, 그 fragment를 Post fragment에서 사용할 것입니다.

  ```
  import { prisma } from '../../../../generated/prisma-client';
  import { POST_FRAGMENT } from '../../../fragments';

  export default {
      Query: {
          seeFullPost: async (_, args, { request, isAuthenticated }) => {
              isAuthenticated(request);
              const { id } = args;
              return await prisma.post({ id }).$fragment(POST_FRAGMENT);
          }
      }
  };

  ```

- fragment.js

  ```
  export const COMMENT_FRAGMENT = `
        id
        text
        user {
            username
        }
  `;

  export const USER_FRAGMENT = `
          id
          username
  `;

  export const FILE_FRAGMENT = `
          id
          url
  `;

  export const POST_FRAGMENT = `
      fragment PostParts on Post {
          id
          caption
          location
          user {
              ${USER_FRAGMENT}
          }
          files {
              ${FILE_FRAGMENT}
          }
          comments {
              ${COMMENT_FRAGMENT}
          }
      }
  `;

  ```
