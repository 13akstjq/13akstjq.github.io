## 리액트 프로젝트 `github` 혹은 `netlify`에 배포하기  



> ## github gh-pages



깃헙에서 제공하는 `gh-pages`를 사용하기 위해서는 `npm`설치가 필요합니다.  

```bash
 npm add gh-pages
```



* build명령어 추가해주기 

  ```js
  "scripts": {
      "start": "react-scripts start",
      "build": "react-scripts build",
      "test": "react-scripts test",
      "eject": "react-scripts eject",
      "deploy": "gh-pages -d build",
      "predeploy": "npm run build"
    },
  ```

* homepage경로 추가해주기 

  ```js
  "homepage": "https://13akstjq.github.io/manflex",
  ```



* 배포하기 

  ```bash
  npm run-script deploy
  ```

  ![image](https://user-images.githubusercontent.com/46010705/61589952-3ae18300-abec-11e9-9163-206c31e2ad61.png)

위 화면으로 접속하면 일정시간 지나면 접속 됩니다.  





> ## netlify 로 배포하기 

front-end를 deploy를 할때 사용하는 플랫폼입니다.  백엔드를 배포하기 위해서는 aws나 goole을 이용해야하는 것 같네요.   

마스터에 `push`를 하면 자동으로 `build`를 해주는 장점이 있습니다.  

* 깃허브로 회원가입

* 원하는 레포지토리 권한 허용 
* 끝



업데이트 하고싶다면 `master`branch에 push 하면 됩니다.  



또한 `pakage.json`파일의 `homepage`에 아래 주소를 넣어줘야 합니다.  

![image](https://user-images.githubusercontent.com/46010705/61590223-7f6f1d80-abf0-11e9-9807-816d6c20a138.png)



`public/index.html`에 아래와 같은 코드가 기본 url 을 정하기 때문입니다.  

```html
<link rel="manifest" href="%PUBLIC_URL%/manifest.json" />
```

