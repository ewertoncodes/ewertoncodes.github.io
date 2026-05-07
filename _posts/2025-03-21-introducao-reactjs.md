---
layout: post
title: Criando meu primeiro componente ReactJS
date: 2025-03-21 00:03:50.000000000 -03:00
categories: frontend reactjs
tags:
  - reactjs
  - components
  - javascript
  - frontend
canonical_url: https://ewertoncodes.github.io/frontend/reactjs/2025/03/21/introducao-reactjs.html
---
O React.js é uma biblioteca JavaScript para criar interfaces dinâmicas na web. Quando desenvolvemos com React, pensamos em componentes, que são pequenos blocos reutilizáveis da interface, como menus, botões e formulários. Basicamente, um componente é uma função que retorna a renderização de um HTML.

No React, usamos JSX, uma sintaxe que permite escrever HTML dentro do JavaScript. Isso facilita muito a construção da interface.

Exemplo de Componente
Aqui está um exemplo de um botão simples feito como componente:

`Button.jsx`

```jsx
export function Button() { 
  return ( 
    <button>Salvar</button> 
  ); 
} 
```

`App.jsx`

```jsx
import { Button } from './components/Button';

export function App() { 
  return ( 
    <div> 
      <Button /> 
    </div> 
    ); 
  } 
```

Esse exemplo é bem básico e com ele vimos como criar um componente no React e utilizá-lo dentro da nossa aplicação. Esse é apenas o primeiro passo para construir interfaces modulares e reutilizáveis. No próximo post, quero falar sobre props, state e como lidar com eventos.
