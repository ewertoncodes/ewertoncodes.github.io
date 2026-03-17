---
layout: post
title:  "Como os Sites Evoluíram"
date:   2025-02-20 00:03:50 -0300
categories: frontend reactjs
---


Este é o meu primeiro post neste blog, e para construí-lo estou usando o **Jekyll**, um gerador de sites estáticos (SSG). Mas, afinal, o que significa um SSG? 🤔

Um **SSG (Static Site Generator)** é uma abordagem onde todas as páginas do site são geradas de forma estática no momento da build, antes mesmo de um usuário acessá-las. Isso significa que, ao invés de processar cada requisição no servidor, o site já está pronto e é entregue como arquivos HTML estáticos. O resultado? Um carregamento muito mais rápido, menor consumo de recursos no servidor .

Mas os sites sempre funcionaram dessa forma? Não! A forma como servimos conteúdo na web evoluiu bastante ao longo do tempo. Antes das abordagens modernas como SPA (Single Page Application) e SSR (Server-Side Rendering), os sites eram renderizados de maneira muito diferente.

## O Início: Sites Tradicionais e o Surgimento do AJAX
No começo da web, a maioria dos sites funcionava com uma abordagem clássica: cada clique em um link ou envio de formulário resultava no carregamento de uma nova página pelo servidor. Isso era simples, mas tornava a navegação lenta e pouco interativa.

A grande revolução veio com o ***AJAX (Asynchronous JavaScript and XML)***. Com essa técnica, os sites puderam buscar e atualizar informações sem precisar recarregar a página inteira. Isso trouxe um nível de interatividade antes impensável.

Um Exemplo famoso que popularizarou o AJAX foi o Gmail. Nele você conseguia visualizar e enviar emails sem precisar atualizar a página.

O AJAX abriu caminho para uma experiência mais dinâmica, mas ainda exigia muitos cuidados no gerenciamento das requisições. Isso levou à evolução para algo ainda mais avançado: as Single Page Applications (SPAs).

## SPAs: O Próximo Passo na Evolução


As **Single Page Applications (SPAs)** levaram a ideia do AJAX ainda mais longe. Em vez de carregar páginas inteiras, elas carregam apenas uma única página inicial e usam JavaScript para atualizar o conteúdo dinamicamente conforme o usuário navega.

Isso tornou a experiência muito mais fluida e interativa, já que não há recarregamento completo da página. Frameworks como React, Vue e Angular popularizaram esse modelo, permitindo que desenvolvedores criassem aplicações altamente dinâmicas.

OK. Mas como funciona uma SPA?

- O usuário acessa a aplicação e o navegador baixa um único arquivo HTML junto com os scripts JavaScript e CSS.
- O JavaScript da página solicita dados do servidor via API (normalmente em JSON).
- A aplicação atualiza dinamicamente o conteúdo exibido, sem precisar recarregar a página inteira.

![image](/images/spa.png)


**Exemplo real**: Quando você navega pelo Gmail, o conteúdo dos e-mails muda sem que a página toda precise ser recarregada. Isso acontece porque o frontend faz chamadas assíncronas para buscar apenas os dados necessários.

Vantagens das SPAs:

 - Navegação rápida após o primeiro carregamento.
 - Melhor experiência do usuário, sem recarregamentos constantes.

Desvantagens das SPAs:

- SEO mais difícil, já que o conteúdo é carregado via JavaScript.
- Tempo de carregamento inicial pode ser maior.

## O Retorno do SSR


O **Server-Side Rendering (SSR)** resolve algumas das limitações das SPAs ao renderizar as páginas no servidor antes de enviá-las ao navegador. Isso melhora o tempo de carregamento e, principalmente, o SEO, já que os motores de busca conseguem indexar melhor o conteúdo já renderizado.

Frameworks modernos como Next.js trouxeram o melhor dos dois mundos, permitindo combinar SSR e SPA dentro do mesmo projeto.

Diferente das SPAs, que carregam um HTML básico e preenchem o conteúdo via JavaScript, o Server-Side Rendering (SSR) gera a página no servidor antes de enviá-la ao navegador. Assim, o usuário recebe um HTML pronto, sem depender do carregamento do JavaScript para visualizar o conteúdo.

Como funciona o SSR?

![image](/images/ssr.png)


- O usuário acessa a aplicação e o navegador faz uma requisição ao servidor.

- O servidor busca os dados (ex.: do banco de dados) e gera um HTML pronto.

- Esse HTML é enviado ao navegador e exibido imediatamente.


**Exemplo no Rails:** Em um app Rails tradicional com ERB Views, cada requisição a uma rota (/posts/1) faz o servidor buscar o post no banco e renderizar a view (show.html.erb) antes de enviá-la ao navegador. Isso garante que o HTML já chegue pronto ao usuário .

Por que o SSR é útil? 

- SEO muito melhor, pois o conteúdo chega pronto para os motores de busca.
- Primeiro carregamento mais rápido.
Por outro lado, o SSR pode aumentar a carga no servidor e exigir um backend mais robusto.

Com o tempo, diferentes abordagens surgiram para melhorar a forma como servimos conteúdo na web. O AJAX trouxe interatividade, as SPAs transformaram a navegação, e o SSG e SSR equilibraram performance e SEO. Hoje, a escolha da abordagem ideal depende das necessidades de cada projeto e das prioridades do desenvolvedor.


