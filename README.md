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

## Estilizando a Navbar
