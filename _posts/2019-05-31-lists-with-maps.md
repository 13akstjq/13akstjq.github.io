---
layout: post
title:  "[react] map을 이용해 쉽게 출력하기  "
date:   2019-05-31 20:00:59
author: 한만섭
categories: react
tags: react state props
---


## lists with maps

리액트에서 배열을 사용해야할 일이 있을때 map을 이용하면 편하다. 


- map을 사용하기 전 코드  
  map을 사용하지 않으면 배열의 길이만큼 코드를 반복해야하는 비효율성이 있다.  
  
  {% highlight html %}
{% raw %}{% highlight javascript %}    
const movieTitles=[
  'matix',
  'full Metal jacket',
  'oldboy',
  'star wars'
]
const movieImages=[
  'https://images-na.ssl-images-amazon.com/images/I/51EG732BV3L._SY445_.jpg',
  'https://i.pinimg.com/736x/36/1e/cd/361ecdb85a3767f70810cbe2cdaaf1a4.jpg',
  'https://upload.wikimedia.org/wikipedia/ko/thumb/4/48/Old_Boy.jpg/250px-Old_Boy.jpg',
  'https://starwarsblog.starwars.com/wp-content/uploads/2015/10/tfa_poster_wide_header-1536x864-959818851016.jpg'
]

class App extends Component {
  render(){
    return (
      <div className="App">
        <Movie title={movieTitles[0]} poster={movieImages[0]}></Movie>
        <Movie title={movieTitles[1]} poster={movieImages[1]}></Movie>
        <Movie title={movieTitles[2]} poster={movieImages[2]}></Movie>
        <Movie title={movieTitles[3]} poster={movieImages[3]}></Movie>
      </div>
    );
  }
}
{% endhighlight %}{% endraw %}
{% endhighlight %}




- map을 사용한 후 코드  
  map을 사용하게 되면  **title**과 **poster**를 key로 가지고 있는 객체 배열을 `map`으로 반복하여  
  `<movie/>`태그를 만들어준다. *movies*객체를 돌면서 *movie*의 **title**과 **poster**를 movie tag의 props로 넘겨준다. 
  {% highlight html %}
{% raw %}{% highlight javascript %}    
const movies = [
  {
    title:"matrix",
    poster:  'https://images-na.ssl-images-amazon.com/images/I/51EG732BV3L._SY445_.jpg'
  },
  {
    title:"full Metal jacket",
    poster:   'https://i.pinimg.com/736x/36/1e/cd/361ecdb85a3767f70810cbe2cdaaf1a4.jpg'
  },{
    title:"oldboy",
    poster:  'https://upload.wikimedia.org/wikipedia/ko/thumb/4/48/Old_Boy.jpg/250px-Old_Boy.jpg'
  },{
    title:"star wars",
    poster:  'https://starwarsblog.starwars.com/wp-content/uploads/2015/10/tfa_poster_wide_header-1536x864-959818851016.jpg'
  }
]
class App extends Component {
  render(){
    return (
      <div className="App">
        {movies.map(movie,index =>{
          return <Movie key={index} title={movie.title} poster={movie.poster}></Movie>
        })}
      </div>
    );
  }
}
{% endhighlight %}{% endraw %}
{% endhighlight %}




