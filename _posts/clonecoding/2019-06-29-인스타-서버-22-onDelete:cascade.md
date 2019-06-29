---
layout: post
title:  "[인스타클론코딩] [Server].21 - prisma에서 cascade 사용하기    "
date:   2019-06-29-16:33:00
author: 한만섭
categories: clonecoding
tags: instagram post cascade
---

> ### 정리할 내용 

MySQL에서 외래키로 묶여있는 테이블을 삭제할 때 사용하는 onDelete Cascade라는 것이 있습니다. 이 강력한 옵션을 사용해서 연관이 되어있는 테이블을 모두 
삭제할 수 있습니다. 그런 기능을 Prisma에서 사용할 수 있는 것을 정리해보려고 합니다.  


> ### cascade

원래 Prisma에 있는 ondelete: cascade에 대해서 정리해보려고 했지만 prisma에서는 연관된 Model에서 삭제가 발생할 경우 default로 setnull을 만들어줍니다. 
따라서 *!(필수기호)*만 없다면 연관된 모델이 삭제되면 자동으로 null이 됩니다. 예를들어 게시물을 삭제하면 댓글들의 post는 null이 되는 것입니다. 
