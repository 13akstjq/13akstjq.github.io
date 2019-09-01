---
layout: post
title:  "[Graphql] GraphQL클라이언트만들기(3)"
date:   2019-06-21-14:02:00
author: 한만섭
categories: graphql
tags: graphql 
---

* TOC
{:toc}


> ### 정리할 내용 
  Graphql을 사용해서 클라이언트를 제작할 경우 Query를 사용하기 때문에 인자가 필요한 경우가 있습니다. 인자를 사용하는 방법을 위주로 정리해보려고 합니다. 
  
  　  
 ***
 
 　  
> ### input값이 없는 query 만들기 
  
  * graphql 서버에서 작성하는 방식으로 작성하면 됩니다. 
  
  ```
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
  
  * 기존의 graphql에서 작성하는 방식과는 약간 다릅니다. **{}**로 **query**를 감싸는 대신에 **query**라는 객체를 만들어서 감싸줍니다. 
    함수명은 상관 없고 인자를 사용할 때는 **$ParameterName**으로 적어주면 됩니다. 
    
  ```
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
  
> ### Query 컴포넌트에서 query 사용하기 


  parameter로 사용하고 싶은 것은 match -> params -> **movieId**입니다. Query태그에 variables={{}}를 넣어줍니다. 여기서 발생했던 이슈는 
  movieId는 String으로 되어있는데 Query에서는 Int!를 받으려고 해서 계속 에러가 발생했습니다. 그래서 movieId = Number(movieId)로 변환을 
  시켜주었습니다. 
  
  ```
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
