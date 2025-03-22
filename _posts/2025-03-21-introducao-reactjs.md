---
layout: post
title:  "Criando meu primeiro componente ReactJS"
date:   2025-03-21 00:03:50 -0300
categories: frontend reactjs
---


O React.js é uma biblioteca JavaScript para criar interfaces dinâmicas na web. Quando desenvolvemos com React, pensamos em componentes, que são pequenos blocos reutilizáveis da interface, como menus, botões e formulários. Basicamente, um componente é uma função que retorna a renderização de um HTML.

No React, usamos JSX, uma sintaxe que permite escrever HTML dentro do JavaScript. Isso facilita muito a construção da interface.

Exemplo de Componente
Aqui está um exemplo de um botão simples feito como componente:

`Button.jsx`

{% highlight jsx %} 
export function Button() { 
  return ( 
    <button>Salvar</button> 
  ); 
} 
{% endhighlight %}

`App.jsx`

{% highlight jsx %} 
import { Button } from './components/Button';

export function App() { 
  return ( 
    <div> 
      <Button /> 
    </div> 
    ); 
  } 
{% endhighlight %}

Esse exemplo é bem básico e com ele vimos como criar um componente no React e utilizá-lo dentro da nossa aplicação. Esse é apenas o primeiro passo para construir interfaces modulares e reutilizáveis. No próximo post, quero falar sobre props, state e como lidar com eventos.
