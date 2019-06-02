---
layout: post
title:  "[react] smart와 dumb비교 "
date:   2019-06-02
author: 한만섭
categories: react
tags: react smart dumb
---


## smart와 dumb 비교 

smart는 `state`를 가지고 있는 component를 의미한다.   
dumb는 `state`를 가지고 있지 않은 component를 의미한다.  

`state`를 가지고 있지 않은 component는 그저 return이 목적이기 때문에 html 코드만 return 해준다.  
그렇기 때문에 class 형태로 선언할 필요가 없다.  

### class 와 function 비교

class
```javascript
class MoviePoster extends Component{
    render(){
        return(
            <img width= "100px" src={this.props.poster}></img>
        );
    }
}

```  


function과 proptype 선언 
```javascript
function MoviePoster({poster}){
    return (
        <img width= "100px" src={poster}></img>
    )
}

MoviePoster.propTypes = {
    poster : PropTypes.string.isRequired
}
```




## class를 function으로 바꾼 결과 
```
import React from 'react';
import PropTypes from 'prop-types';
import './Movie.css';

function Movie({title , poster}){
    return(
        <div>
            <MoviePoster poster={poster}></MoviePoster>
            <h1>{title}</h1>
        </div>
    )
}

function MoviePoster({poster}){
    return (
        <img width= "100px" src={poster}></img>
    )
}

Movie.propTypes = {
    title : PropTypes.string.isRequired,
    poster : PropTypes.string.isRequired
}

MoviePoster.propTypes = {
    poster : PropTypes.string.isRequired
}
export default Movie;

```
