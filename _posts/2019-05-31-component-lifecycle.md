---
layout: post
title:  "[react] react lifecycle  "
date:   2019-05-31 21:00:59
author: 한만섭
categories: react
tags: react state props
---




## 리액트의 생명주기 

```
class App extends Component {
  render(){
    return (
      <div className="App">
        {movies.map(movie,index =>{
          return <Movie key={index} title={movie.title} poster={movie.poster}></Movie>
        })}
        <Movie title={movieTitles[0]} poster={movieImages[0]}></Movie>
        <Movie title={movieTitles[1]} poster={movieImages[1]}></Movie>
        <Movie title={movieTitles[2]} poster={movieImages[2]}></Movie>
        <Movie title={movieTitles[3]} poster={movieImages[3]}></Movie>
      </div>
    );
  }
}
```
