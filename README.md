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

## Estilizando o moviegrid.sass

> Importamos o moviegrid.sass na Home.jsx

```jsx
import "../styles/pages/moviegrid.sass";
```

> Criamos o mixin para o button

mixins.sass

```sass

@use './variables'

@mixin button
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

> Estilização do moviegrid.sass

```sass
@use '../variables'
@use '../mixins'

.container .title
  color: #fff
  font-size: 2.5rem
  text-align: center
  margin: 2rem 0 1rem

.title .query-text
  color: variables.$bg-color-yellow

.movies-container
  display: flex
  flex-wrap: wrap
  justify-content: space-between
  padding: 2rem
  max-width: 1200px
  margin: 0 auto
  text-align: center

  div
    width: 30%
    color: #fff
    margin-bottom: 2.5rem
    display: flex
    flex-direction: column
    justify-content: space-between
    background-color: #111
    padding: 1rem

  svg
    color: variables.$bg-color-yellow
  img
    max-width: 100%
  img,h2,p
    margin-bottom: 1rem
  a
    @include mixins.button
    padding: 1rem .5rem
    text-align: center

    &:hover
      color: variables.$bg-color-yellow


```

## Fazendo a parte de pesquisa

Navbar.jsx

> Criamos o estado do search

```tsx
const [search, setSearch] = useState("");
```

> Criamos uma variavel que navega para tal pagina

```jsx
const navigate = useNavigate();
```

> Criamos uma funcao que pega o submit do formulário
> passamos evento preventDefault para nao carregar a pagina
> Passamos um if se nao houver valor no search nao faz nada
> Passamos na funcao navigate a Url com o valor pesquisado o input
> Limpamos a variavel do estado search

```jsx
const handleSubmit = (e) => {
  e.preventDefault();

  if (!search) return;

  navigate(`/search?q=${search}`);
  setSearch("");
};
```

> Passamos onChange e setando o valor colocado no input
> colocamos um value nele o estado search para limparmos o input

```jsx
<input
  type="text"
  placeholder="Busque um filme"
  onChange={(e) => setSearch(e.target.value)}
  value={search}
/>
```

## Agora vamos criar a pagina do search

> Copiamos o JSX da Home e modificamos

```jsx
<div className="container">
  <h2 className="title">
    // mudamos o titulo e passamos a query Resultados para:{" "}
    <span className="query-text">{query}</span>
  </h2>
  <div className="movies-container">
    // mudamos o nome para movies
    {movies.length == 0 && <p>Carregando...</p>}
    {movies.length > 0 &&
      movies.map((movie) => <MovieCard movie={movie} key={movie.id} />)}
  </div>
</div>
```

> importamos os estados
> importamos o useSearchParams para pegar pela url
> importamos o componente MoviesCard
> Pegamos as variáveis VITE_SEARCH e VITE_API_KEY e passamo em uma variavel

```jsx
import { useState, useEffect } from "react";
import { useSearchParams } from "react-router-dom";
import { MovieCard } from "../components/MoviesCard";

const searchURL = import.meta.env.VITE_SEARCH;
const apiKey = import.meta.env.VITE_API_KEY;
```

> importamos o estilo moviegrid.sass

```jsx
import "../styles/pages/moviegrid.sass";
```

> Criamos uma variavel destruturando ela usando useSearchParams

```jsx
const [searchParams] = useSearchParams();
```

> Criamos o estado do movies

```tsx
const [movies, setMovies] = useState([]);
```

> criamos uma variavel para pegar a variavel da url

```jsx
const query = searchParams.get("q");
```

> Copiamos da Home a funcao que pega os filmes e modificamos ela

```jsx
const getSearchedMovies = async (url) => {
  const res = await fetch(url);
  const data = await res.json();

  setMovies(data.results);
};

useEffect(() => {
  const searchWithQueryURL = `${searchURL}?${apiKey}&query=${query}`;

  getSearchedMovies(searchWithQueryURL);
}, [query]);
```

## Estruturando a pagina de Movie.jsx

> Importando useState useEffect useParams e os icones

```jsx
import { useState, useEffect } from "react";
import { useParams } from "react-router-dom";
import {
  BsGraphUp,
  BsWallet2,
  BsHourglassSplit,
  BsFillFileEarmarkTextFill,
} from "react-icons/bs";
```

> criamos um arquivo movie.sass e importamos ele

```jsx
import "../styles/pages/movie.sass";
```

> importamos nosso MovieCard

```jsx
import { MovieCard } from "../components/MoviesCard";
```

> importamos nossa variáveis a URL

```jsx
const moviesURL = import.meta.env.VITE_API;
const apiKey = import.meta.env.VITE_API_KEY;
```

> Criamos uma funcao Movie

export const Movie = () => {

> Usamos o useParams e pegamos o id que vem pela url

const { id } = useParams();

> Criamos o estado dos filmes

const [movie, setMovie] = useState(null);

> Criamos uma funcao que pega os filmes

const getMovie = async (url) => {

> Pegamos usando o fetch

const res = await fetch(url);

> pegamos o res e passamos a funcao json

const data = await res.json();

> Setamos a data no estado setMovie

    setMovie(data);

};

> Criamos uma funcao que transforma o numero em dolar

const FormatCurrency = (number) => {
return number.toLocaleString("en-US", {
style: "currency",
currency: "USD",
});
};

> passamos o estado para a funcao useEffect

useEffect(() => {

> criamos uma variavel que pega a url o id e passamo a chave key bele

const movieUrl = `${moviesURL}${id}?${apiKey}`;

> passamo a funcao com a url

getMovie(movieUrl);
}, []);

> criamos o JSX do movie

```jsx
return (
  <div>
    {movie && (
      <>
        <MovieCard movie={movie} showLink={false} />
        <p className="tagline">{movie.tagline}</p>
        <div className="info">
          <h3>
            <BsWallet2 /> Orçamento:
          </h3>
          <p>{FormatCurrency(movie.budget)}</p>
        </div>
        <div className="info">
          <h3>
            <BsGraphUp /> Receita:
          </h3>
          <p>{FormatCurrency(movie.revenue)}</p>
        </div>
        <div className="info">
          <h3>
            <BsHourglassSplit /> Duração:
          </h3>
          <p>{movie.runtime} minutos </p>
        </div>
        <div className="info description">
          <h3>
            <BsFillFileEarmarkTextFill /> Descrição
          </h3>
          <p>{movie.overview}</p>
        </div>
      </>
    )}
  </div>
);
```

## Estilizando movie.sass

```sass
@use '../variables'
@use '../mixins'

.movie-page
  color: #fff
  display: flex
  flex-direction: column
  max-width: 600px
  margin: 2rem auto

  svg
    font-size: 1.5rem
    color: variables.$bg-color-yellow

  .movie-card
    text-align: center

    img, h2, p
      margin-bottom: 1rem

  .movie-card h2
    font-size: 2rem
  .movie-card p
    display: flex
    align-items: center
    justify-content: center
    gap: .4rem

.tagline
  text-align: center
  font-size: 1.3rem
  margin-bottom: 2rem

.info
  margin-bottom: 1.5rem

  h3
    margin-bottom: 1rem
    display: flex
    align-items: center
    gap: .5rem

.description
  padding-bottom: 15rem

  p
   line-height: 1.4rem


```
## FIM