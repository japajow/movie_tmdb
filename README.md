## Matheus Batisti Movies TMDB

> instalando com vite o react
> npm create vite@latest
> movie_tmdb
> react
> react

> Instalando os pacotes que vamos usar no projeto
> npm install react-icons
> npm i react-router-dom

> Startando o projeto
> npm run dev

> Instalando as variáveis de ambiente
> Criando o arquivo .env

> Colocando as chaves do API no projeto usando .env

```env

VITE_API_KEY=api_key=chave_vai_aqui
VITE_API=https://api.themoviedb.org/3/movie/
VITE_SEARCH=https://api.themoviedb.org/3/search/movie/
VITE_IMG=https://api.themoviedb.org/t/p/w500

```

> Configurando o react router

main.jsx

> importamos

```jsx
import { BrowserRouter, Routes, Route } from "react-router-dom";
```

> Utilizando os Routes

```tsx
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";
import { BrowserRouter, Routes, Route } from "react-router-dom";

import "./index.css";

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <BrowserRouter>
      <Routes>
        <Route element={<App />}>
          <Route path="/" element={<Home />} />
          <Route path="/movie/:id" element={<Movie />} />
          <Route path="/search" element={<Search />} />
        </Route>
      </Routes>
    </BrowserRouter>
  </React.StrictMode>
);
```

> Criando os componentes Home Movie e Search

src/pages/Home.jsx
src/pages/Movie.jsx
src/pages/Search.jsx

> Estruturando cada pages

```jsx
export const Home = () => {
  return <div>Home</div>;
};
```

```jsx
export const Movie = () => {
  return <div>Movie</div>;
};
```

```jsx
export const Search = () => {
  return <div>Search</div>;
};
```

> Criando um navbar para verificar se as rotas estão funcionando

App.jsx

```jsx
<nav id="navbar">
  <h2>
    <Link to="/">moviesLib</Link>
  </h2>
  <Link to="/movie/1">Movie</Link>
  <Link to="/search">Search</Link>
</nav>
<h2>Movie lib</h2>
<Outlet />
```

## Criando os componentes

src/components/Navbar.jsx

> Cortamos a navbar que criamos no App.jsx e colamos no Navbar.jsx

Navbar.jsx

```jsx
import { Link } from "react-router-dom";

export const Navbar = () => {
  return (
    <nav id="navbar">
      <h2>
        <Link to="/">moviesLib</Link>
      </h2>
      <Link to="/movie/1">Movie</Link>
      <Link to="/search">Search</Link>
    </nav>
  );
};
```

> Importamos a Navbar.jsx no app.jsx

```jsx
function App() {
  return (
    <div className="App">
      <Navbar />
      <h2>Movie lib</h2>
      <Outlet />
    </div>
  );
}

export default App;
```

> Estruturando o navbar

Navbar.jsx

```jsx
import { Link } from "react-router-dom";
import { BiCameraMovie, BiSearchAlt2 } from "react-icons/bi";

export const Navbar = () => {
  return (
    <nav id="navbar">
      <h2>
        <Link to="/">
          <BiCameraMovie />
        </Link>
      </h2>
      <form>
        <input type="text" placeholder="Busque um filme" />
        <button type="submit">
          <BiSearchAlt2 />
        </button>
      </form>
    </nav>
  );
};
```

## Estilizando o projeto

> Decidi usar o sass para dar continuidade no nosso projeto feito com sass

> criando pasta styles

> criando arquivos
> styles/main.sass
> styles/mixins.sass
> styles/variables.sass

> Colocando estilos default no projeto
> main.sass

```sass
*
 font-family: "Helvetica"
 margin: 0
 padding: 0
 box-sizing: border-box

body
 background-color: #000

a ,svg
 text-decoration: none
 color: #f7d354
 transition: .5s

 &:hover
   color: #b8930c

```

## ESTRUTURANDO A HOME

> importando os hooks State e useEffect

```jsx
import { State, useEffect } from "react";
```

> importando as variáveis

```jsx
const moviesURL = import.meta.env.VITE_API;
const apiKey = import.meta.env.VITE_API_KEY;
```

> Criamos os estados do movie

```jsx
const [topMovies, setTopMovies] = useState([]);
```

> Criamos uma funcao que pega os dados usando fetch async await

```jsx
const getTopRatedMovies = async (url) => {
  const res = await fetch(url);
  const data = await res.json();
};
```

> Usamos o useEffect para quando os dados estiverem prontos exibimos ele

```jsx
useEffect(() => {
  const topRatedURL = `${moviesURL}top_rated?${apiKey}`;

  // Passamos o getTopRatedMovies passando a URL
  getTopRatedMovies(topRatedURL);
}, []);
```

> Passamos o setTopMovies a data.results
> Assim preenchemos os dados dos filmes no topMovies

```jsx
const getTopRatedMovies = async (url) => {
  const res = await fetch(url);
  const data = await res.json();

  setTopMovies(data.results);
};
```

> Imprimir os filmes fazendo um looping em um componente
> testando se vem os nomes dos filmes

```jsx
return (
  <div>{topMovies && topMovies.map((movie) => <h2>{movie.title}</h2>)}</div>
);
```

> Estruturando a Home

```jsx
return (
  <div className="container">
    <h2 className="title">Melhores files:</h2>
    <div className="movies-container">
      {topMovies.length == 0 && <p>Carregando...</p>}
      {topMovies.length > 0 && topMovies.map((movie) => <h2>{movie.title}</h2>)}
    </div>
  </div>
);
```

## Criando o componente de filmes

> Importamos o Link e o icone FaStar

```tsx
import { Link } from "react-router-dom";

import { FaStar } from "react-icons/fa";
```

> importamos a variavel que pega a url da imagem

```tsx
const imageURL = import.meta.env.VITE_IMG;
```

> Passamos os parâmetros que vamos utilizar

```tsx
export const MovieCard = ({ movie, showLink = true }) => {
  return <div>MovieCard</div>;
};
```

> Estruturando o JSX da Home

```tsx
export const MovieCard = ({ movie, showLink = true }) => {
  return (
    <div className="movie-card">
      // passamos a URL da imagem concatenando como movie.poster_path
      <img src={imageURL + movie.poster_path} alt={movie.title} />
      // Passamos o titulo
      <h2>{movie.title}</h2>
      <p>
        //Chamamos o ícone
        <FaStar />
        // Passamos a pontuação
        {movie.vote_average}
      </p>
      // Caso o Tiver o link mostramos o botão
      {showLink && <Link to={`/movie/${movie.id}`}>Detalhes</Link>}
    </div>
  );
};
```

## Passando o componente Movie na Home

Home.jsx

```tsx
{
  topMovies.length > 0 &&
    topMovies.map((movie) => <MovieCard movie={movie} key={movie.id} />);
}
```

> Estilizando a Home

> Criamos o styles/components/arquivo navbar.sass

```sass
@use '../variables'
@use '../mixins'

#navbar
  display: flex
  justify-content: space-between
  align-items: center
  padding: 1rem 2rem
  background-color: #121212

  h2 a
    display: flex
    align-items: center
    gap: .5rem

  form
    display: flex
    gap: .5rem

  input
    padding: .2rem .8rem
    border-radius: 4px
    border: none

  button
    background-color: variables.$bg-color-yellow
    border: 2px solid variables.$bg-color-yellow
    border-radius: 4px
    color: #000 !important
    padding: .3rem
    font-size: 1.3rem
    display: flex
    align-items: center
    cursor: pointer
    transition: .4s

    &:hover
      background-color: transparent
      svg
        color: variables.$bg-color-yellow

```


