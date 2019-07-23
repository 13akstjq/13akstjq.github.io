---
layout: post
title:  "[Git] Git Rebase 정리 "
date:   2019-07-24-00:20:00
author: 한만섭
categories: git
tags: git github rebase issue 
---


> ##  Git Rebase 사용해보기 

평소에는 `master`branch에서 새로운 `branch`를 만들어서 작성을 하다가 기능이 완성되면 `master`에서 `merge`를 하는 방식으로 개발을 했었는데, 이번에 팀원들과 프로젝트를 진행하면서 `rebase`와 `squash` `issue`를 사용해서 좀 더 깊이 있게 사용해보려고 합니다.  



> ### Rebase란 ?

`re + base`말 그대로 base를 재설정 한다는 의미입니다.  

[참고사이트](<https://cyberx.tistory.com/96>)

![image](https://user-images.githubusercontent.com/46010705/61695458-7ad07380-ad6e-11e9-91b4-1bc15c52896c.png)


위와 같이 `master branch`와 3개의 `commit`객체가 있는 상황에서 

```bash
git checkout -b dev 
```

위 명령어를 통해서 새로운 `dev branch`를 만들고 `dev branch`로 `checkout`을 하고,  커밋 명령어를 통해서 2번 `commit`을 했다고 가정해보겠습니다. 그러면 아래와 같은 그래프를 가질 것입니다.  



![image](https://user-images.githubusercontent.com/46010705/61695738-01855080-ad6f-11e9-960b-47def2045d58.png)


위 상황에서 `dev branch`를 `master branch`로 병합하는 명령어를 실행해봅니다.  

```bash
git checkout master
git merge dev
```

![1563870476869](C:\Users\multicampus\AppData\Roaming\Typora\typora-user-images\1563870476869.png)

`commit2`를 가르키고 있던 `master` branch가 `merge`를 통해서 `fast-forward`되어서 `dev`와 같은 `branch`를 가리킵니다.  


이렇게 보면 `merge`만 사용해도 이상적인 commit graph를 나타낸다고 생각할 수 도 있습니다. 하지만 제가 branch를 만들어서 작업을 하는 도중 팀원이 커밋을 진행했을 경우 아래와 같은 그래프를 나타납니다.  

![image](https://user-images.githubusercontent.com/46010705/61696080-a30ca200-ad6f-11e9-9c0b-aff969bbebac.png)

위 상황에서 `master`로 `checkout`을 해서 `merge`를 해주게 되면 그래프가 복잡해지는 상황이 나옵니다.  

```bash
git checkout master
git merge dev
```

![1563870725722](C:\Users\multicampus\AppData\Roaming\Typora\typora-user-images\1563870725722.png)

이렇게 하게되면 `fast-forward`가 아닌 새로운 merge commit을 남기게 됩니다.  



## Rebase

그렇다면 `rebase`를 사용하면 어떻게 되는지 확인해보겠습니다.  

![1563870845179](C:\Users\multicampus\AppData\Roaming\Typora\typora-user-images\1563870845179.png)

글의 처음에서 얘기했듯이 `rebase`란 base를 다시 지정하다라는 의미입니다. 현재 `dev` branch의 root는 `commit2`로서 `master`가 가르키고 있는 `commit`보다 뒤에 위치합니다.  



여기서 아래와 같은 명령어를 사용해봅니다.  

```bash
git checkout dev
git rebase master
```

위 명령어는 `dev` branch의 base를 `master`로 사용하겠다는 의미입니다.  

![1563871040273](C:\Users\multicampus\AppData\Roaming\Typora\typora-user-images\1563871040273.png)

그러면 위와 같은 형태의 그래프가 나타나게 됩니다.  



***



> ## 내 프로젝트에 적용해보기 



제가 하는 프로젝트는 master 밑에 `develop` branch를 사용하기 때문에 develop을 기준으로 작성하려고 합니다.  

1. branch 확인하기 

   ```bash
   git branch -a
   ```

2. develop branch로 checkout 하기 

   ```bash
   git checkout develop
   ```

3. 원하는 `feature/작업명` branch만들기 

   ```bash
   git checkout -b feature/작업명 
   ```

4. 작업 후 commit 하기 

   ```bash
   git add .
   git commit -m "mansub | 커밋명"
   ```

5. 원격 develop에서 pull하면서 rebase받기  

   ```bash
   git pull --rebase origin develop
   ```

6. 원격 `feature` branch에 push 하기 

   ```bash
   git push origin feature/작업명
   ```

7. `dev` branch 에 checkout 하기 

   ```bash
   git checkout develop
   ```

8. 내가 rebase 해서 갱신된 develop을 local develop에 pull하기 

   ```bash
   git pull origin develop
   ```

9. 작업했던 feature branch 삭제하기 

   ```bash
   git branch -D feature/작업명
   ```

9. 삭제했다는 것을 원격에 알리기 

   ```bash
   git push origin --delete feature/작업명
   ```

   

   

