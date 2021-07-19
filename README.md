# Movie App

Nomad Coders에 ReactJS로 영화 웹 서비스 만들기를 보면서 만든 Movie App입니다. 
https://goat95.github.io/movie-app/#/

## 배운점
React를 사용하여 api에서 데이터를 가져오는 방법과 component life cycle, router 사용하는 방법을 배웠습니다. 

## 전체적인 구조
1. components
    * Movie.js - 영화 이미지, 제목, 설명, 장르, 년도등 정보를 담고있는 컴포넌트입니다.
    * Navigation.js - 컴포넌트 이름 그대로 네비게이션 컴포넌트입니다.
2. routes
    * Home.js - Home 페이지를 라우팅 해주는 파일입니다.
    * About.js - About 페이지를 라우팅 해주는 파일입니다.
    * Detail.js - Detail 페이지를 라우팅 해주는 파일입니다.
## 컴포넌트 파일들 설명
### Movie.js
```javascript
import React from 'react';
import { Link } from 'react-router-dom';
import './Movie.css';

function Movie({ id, year, title, summary, poster, genres }) {
  return (
    <div className="movie">
      {/* movie detail 페이지로 모든 props를 보낸다. */}
      <Link
        to={{
          pathname: `/movie/${id}`,
          state: {
            year,
            title,
            summary,
            poster,
            genres
          }
        }}
      >
        <img src={poster} alt={title} title={title} />
        <div className="movie__data">
          <h3 className="movie__title">{title}</h3>
          <h5 className="movie__year">{year}</h5>
          {/* map을 이용해서 genre를 li태그로 하나씩 출력 */}
          <ul className="movie__genres">
            {genres.map((genre, index) => (
              <li key={index} className="genres__genre">
                {genre}
              </li>
            ))}
          </ul>
          {/* summary가 영화마다 다르기 때문에 맞춰주기 위해 slice를 이용해서 180자 까지만 출력 */}
          <p className="movie__summary">{summary.slice(0, 180)}...</p>
        </div>
      </Link>
    </div>
  );
}

export default Movie;
```
### Navigation.js
```javascript
import React from "react";
import { Link } from "react-router-dom";
import "./Navigation.css";

function Navigation() {
  return (
    <div className="nav">
      {/* Link는 href가 아니라 to를 이용해야 한다. */}
      <Link to="/">Home</Link>
      <Link to="/about">About</Link>
    </div>
  );
}

export default Navigation;
```
## 라우트 파일들 설명
### Home.js
```javascript
import React from 'react';
import axios from 'axios';
import Movie from '../components/Movie';
import './Home.css';

class Home extends React.Component {
  state = {
    isLoading: true,
    movies: []
  };
  // 영화를 받아오는 함수를 비동기적으로 실행하겠다.
  getMovies = async () => {
    // fetch 대신 axios를 이용해서 영화 리스트를 rating순으로 정렬해서 받아온다.
    const { data: { data: { movies } } } = await axios.get("https://yts-proxy.nomadcoders1.now.sh/list_movies.json?sort_by=rating");
    // 영화 리스트를 await을 이용하여 받아오면 isLoading을 false로 state를 변경한다.
    this.setState({ movies, isLoading: false });
  }
  componentDidMount() {
    this.getMovies();
  }
  render() {
    const { isLoading, movies } = this.state;
    return (
      <section className="container">
        {isLoading ? (
          <div className="loader">
            <span className="loader_text">Loading...</span>
          </div> 
          ) : (
            <div className="movies">
              {/* Movie 컴포넌트에서 props를 받아서 map을 이용해서 영화 리스트를 리턴한다. */}
              {movies.map(movie => (<Movie key={movie.id} id={movie.id} year={movie.year} title={movie.title} summary={movie.summary} poster={movie.medium_cover_image} genres={movie.genres} />))}
            </div>
          )}
      </section>
    );
  }
}

export default Home;
```
### About.js
```javascript
import React from "react";
import "./About.css";

// route를 사용하면 route props를 사용할 수 있다.
function About(props) {
  console.log(props);
  return (
    <div className="about__container">
      <span>
        “Freedom is the freedom to say that two plus two make four. If that is
        granted, all else follows.”
      </span>
      <span>− George Orwell, 1984</span>
    </div>
  );
}

export default About;
```
### Detail.js
```javascript
import React from 'react';

class Detail extends React.Component {
  componentDidMount() {
    // route props에서 location과 history를 사용.
    const { location, history } = this.props;
    if (location.state === undefined) {
      // props에 history안에 push를 이용해서 Home으로 리다이렉트 시킨다.
      // Detail 페이지로 갈 수 있는 유일한 방법은 poster를 클릭해야만 갈 수 있다.  
      history.push("/");
    }
  }
  render() {
    const { location } = this.props;
    if (location.state) {
      // Movie 컴포넌트에서 전달받은 title props를 이용해서 title 출력
      return <span>{location.state.title}</span>
    } else {
      return null;
    }
  }
}
export default Detail;
```
