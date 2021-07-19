# Movie App

React를 사용하여 만든 Movie App입니다.
https://goat95.github.io/movie-app/#/

## 전체적인 구조
1. components
    * Movie.js - 영화 이미지, 제목, 설명, 장르, 년도등 정보를 담고있는 컴포넌트입니다.
    * Navigation.js - 컴포넌트 이름 그대로 네비게이션 컴포넌트입니다.
2. routes
    * Home.js - Home 페이지를 라우팅 해주는 파일입니다.
    * About.js - About 페이지를 라우팅 해주는 파일입니다.
    * Detail.js - Detail 페이지를 라우팅 해주는 파일입니다.
## 컴포넌트 자세한 설명
### Movie.js
```javascript
import React from 'react';
import { Link } from 'react-router-dom';
import './Movie.css';

function Movie({ id, year, title, summary, poster, genres }) {
  return (
    <div className="movie">
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
          <ul className="movie__genres">
            {genres.map((genre, index) => (
              <li key={index} className="genres__genre">
                {genre}
              </li>
            ))}
          </ul>
          <p className="movie__summary">{summary.slice(0, 180)}...</p>
        </div>
      </Link>
    </div>
  );
}

export default Movie;
```
