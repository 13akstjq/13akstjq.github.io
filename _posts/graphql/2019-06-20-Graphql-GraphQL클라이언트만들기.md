---
layout: post
title:  "[Graphql] GraphQL클라이언트만들기 & react-apollo를 사용해 데이터 요청하기"
date:   2019-06-20-19:04:00
author: 한만섭
categories: graphql
tags: graphql apollo boost
---

* TOC
{:toc}
## 1. GraphQL 클라이언트 프로젝트 만들기
  **Graphql**을 통해서 RESTAPI와 같은 API서버를 만들었다면 서버에게 데이터를 요청할 클라이언트(front)부분이 필요합니다. 오늘은 클라이언트 부분을 
  만드는 방법을 정리해보려 합니다. Apollo를 사용해서 프로젝트를 만드려고 합니다. 

### 1.1 프로젝트 만들기 
우선 프로젝트를 만들어야 합니다. 기본 CRA 프로젝트 setting 하는 방법은 항상 동일합니다.  
- 콘솔에서 **CRA**프로젝트 만들어주기  

- **Git Repository**만들기  

- CRA프로젝트에서 필요없는 파일들 **지우기**  

- CRA와 Git **연동하기**  
  이렇게 하면 기본적인 프로젝트 생성은 완성입니다.  

***

　  

### 1.2 라이브러리 설치하기 

  * react-router 설치하기 
  
    ```
    npm add react-router-dom
    ```
  
  * react Apollo 설치하기 
  
    react-apollo는 [github](https://github.com/apollographql/react-apollo)에 있는 커맨드를 입력해 설치를 해야합니다.  
    **Apollo-boost**는 GraphQL Client를 사용하기위해 모든 것을 대신 셋업해주는 라이브러리라고 생각하시면 됩니다.    

    ```bash
    # installing the preset package (apollo-boost) and react integration
    npm install apollo-boost react-apollo graphql-tag graphql --save

    # installing each piece independently
    npm install apollo-client apollo-cache-inmemory apollo-link-http react-apollo graphql-tag graphql --save
    ```
    

***



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



### 1.3 apolloClient.js 만들기 

   * ApolloClient.js
      **uri**에 적는 주소는 **GraphQL**서버의 주소입니다. 저는 localhost:4000에 켜놨기 때문에 아래와 같이 작성했습니다. 
      ```js
      import ApolloClient from 'apollo-boost';

      const client = new ApolloClient ({
          uri :"http://localhost:4000/"
      })

      export default client;
      ```
      
  * App.js
    ```js
    import React ,{Component}from 'react';
    import {ApolloProvider} from 'react-apollo';
    import client from './apolloClient';

    class App extends Component {
      render(){
        return (
          <ApolloProvider client={client}>
            <div className="App"/>
          </ApolloProvider>
        );
      }
    }
    export default App;
    ```

***



### 1.4 테스트 



**npm start**로 실행시키면 아래와 같은 화면이 나옵니다.  
![image](https://user-images.githubusercontent.com/46010705/59844159-1dbf5780-9395-11e9-928b-beb2c9ddd0de.png)
화면에서 사용한 tool은 [apollo dev tools](https://chrome.google.com/webstore/detail/apollo-client-developer-t/jdkknkkbebbapilgoeccciglkfbmbnfm)에서 설치할 수 있습니다.  


​    

***



## 2. React-apollo 를 이용한 query요청 보내기 



Graphql을 사용해서 클라이언트를 제작할 경우 Query를 사용하기 때문에 인자가 필요한 경우가 있습니다. 인자를 사용하는 방법을 위주로 정리해보려고 합니다. 

------

### 2.1 파라메터가 없는 query 만들기 

graphql 서버에서 작성하는 방식으로 작성하면 됩니다. 

```js
export  const HOME_PAGE = gql`
  {
movies(limit:20,rating:8){
  id
  title
  summary
  genres
  rating
  medium_cover_image
   }
  }
    `;
```


### 2.2  파라메터가 있는 query 만들기 

기존의 graphql에서 작성하는 방식과는 약간 다릅니다. **{}**로 **query**를 감싸는 대신에 **query**라는 객체를 만들어서 감싸줍니다. 
함수명은 상관 없고 인자를 사용할 때는 **$ParameterName**으로 적어주면 됩니다. 

```js
export const MOVIE_DETAILS = gql`
  query getMovieDetails($movieId: Int!) {
    movie(id: $movieId) {
      medium_cover_image
      title
      rating
      language
      genres
    }
    suggestion(id: $movieId) {
      title
      rating
      medium_cover_image
    }
  }
`;
```



***



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



### 2.3 react-apollo 의 Query컴포넌트를 사용해 query요청하기 

parameter로 사용하고 싶은 것은 match -> params -> **movieId**입니다. Query태그에 variables={{}}를 넣어줍니다.

* 이슈 :  movieId는 String으로 되어있는데 Query에서는 Int!를 받으려고 해서 계속 에러가 발생했습니다. 그래서 movieId = Number(movieId)로 변환을 시켜주었습니다. 

```js
  const Detail = ({match : {params : {movieId}}}) => {
    movieId = Number(movieId); // String 형태로 들어오는데 int 형태가 query 문에서는 필요함. 
    return <Query query={MOVIE_DETAILS} variables={{movieId}}>{({loading, data, error}) =>{
        if(loading)return <span>loading</span>;
        if(error) return    <span>error</span>;
        return (
            <DetailContainer>
                <h1>Details</h1>
                <MovieDetail>
                    <Poster img={data.movie.medium_cover_image}></Poster>
                    <Desc>
                        <h1>제목 : {data.movie.title}</h1>
                        <h3>평점 : {data.movie.rating}⭐️</h3>
                        <h3>언어 : {data.movie.language}</h3>
                        <h3>장르 : {data.movie.genres}</h3>
                    </Desc>
                    
                </MovieDetail>
                <h1>Suggestions</h1>
                <Suggestions>
                    <Poster img={data.suggestion[0].medium_cover_image}></Poster>
                    <Poster img={data.suggestion[1].medium_cover_image}></Poster>
                    <Poster img={data.suggestion[2].medium_cover_image}></Poster>

                </Suggestions>
            </DetailContainer>
        );
    }}</Query>
}
```



***



## 3. 요약



- ApolloProvider** 와 **react-router-dom**을 이용해 상태에 따라 URL 맵핑을 해주는 작업을 해야합니다.  

  - **HashRouter**를 **Router**로 사용하고 실제 경로를 변경 시켜줄 **Route**를 import 해줍니다.  
  - **ApolloProvider**를 이용해 Client를 제공해야하기 때문에 역시 import 해야합니다.  
  - Provider에 연결할 만들었던 **Client**를 import 합니다.  
  - 경로에 필요한 **HOME**과 **Detail**을 import해줍니다. 

  흐름 : `Client`를 만들어서 `Provider` 에 연결 후 `Router`안에 실제 경로인 `Route`를 넣어줌 Route안에는 `Query`를 날리는 `Component`가 있음. 

  ```js
    import React ,{Component}from 'react';
    import {HashRouter as Router,Route} from 'react-router-dom';
    import {ApolloProvider} from 'react-apollo';
    import client from './apolloClient';
    import Home from './Home';
    import Detail from './Detail';
    import styled from 'styled-components';
    
    class App extends Component {
      render(){
        return (
          <ApolloProvider client={client}>
            <Router>
              <Container >
                <Route exact={true}  path = {'/'} component={Home}></Route>
                <Route path = {'/details/:movieId'} component={Detail}></Route>
              </Container>
            </Router>
            {/* <div className="App"/> */}
          </ApolloProvider>
        );
      }
    }
  ```

- queries.js

  query를 만들 때는 `graphql-tag`사용합니다. 쿼리를 작성하는 방식은 styled component와 비슷합니다. 

  ```js
  import gql from 'graphql-tag';
  
  export  const HOME_PAGE = gql`
      {
    movies(limit:20,rating:8){
      id
      title
      summary
      genres
      rating
      medium_cover_image
       }
      }
  `
  ```

- Home.js

  - 여기서는 query를 직접 import 해서 사용해야합니다. `react-apollo`에서 제공하는 query 태그에 제작한 HOME_PAGE라는 query를 넣습니다. 
    그러면 `Query`는 loading, data, error 세가지를 리턴해주는데 이것에 맞는 코딩을 하면 됩니다. ( return 필요)
  - Movie 컴포넌트를 만들고 mapping 하는 방법을 까먹어서 잘 기억해놔야 할것 같습니다. 

  ```js
  import React from 'react';
  import {Query} from 'react-apollo';
  import {HOME_PAGE} from './queries';
  import Movie from './Movie';
  import styled from 'styled-components';
  
  const Loading = styled.span`
      font-size : 40px;
      font-weight : 600;
  `
  
  const Error = styled.span`
      font-size : 40px;
      font-weight : 600;
      color : red;
  `
  
  const Home = () => <Query query={HOME_PAGE}>{({loading, data, error}) => {
      if(loading) return  <Loading>loading...</Loading>
      if(error) return <Error>Error!!!</Error>
      return data.movies.map(movie => <Movie key= {movie.id} title={movie.title} img={movie.medium_cover_image} rating={movie.rating} id={movie.id}></Movie>
  );
  }}</Query>;
  export default Home;
  ```

- Movie.js

  마지막으로 볼 코드는 Movie인데 여기서는  영화를 눌렀을 경우 상세보기로 가야하는데 그 이벤트를 **react-router-dom**의 **Link**를 사용했는 것
  말고는 없는 것 같습니다. 

  ```js
     class Movie extends Component {
  
      render(){
          const {title,rating,img,id} = this.props;
          return (
              <Link to={`/details/${id}/`}>
                  <Card img={img}>
                      <Title>{title} / {rating}⭐️</Title>
                  </Card>
              </Link>
          );
      }
  }
  ```

  