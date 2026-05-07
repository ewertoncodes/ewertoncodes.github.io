---
layout: post
title: 'O problema N+1: Como identificar e resolver'
date: 2026-02-23 00:17:30.000000000 -03:00
categories:
- backend
- performance
tags:
- bancodedados
- performance
- ruby
- rails
- sql
description: Entenda o que é o temido problema de performance N+1, como ele afeta
  seu banco de dados e as melhores estratégias para resolvê-lo no Ruby on Rails.
canonical_url: https://ewertoncodes.github.io/backend/performance/2026/02/23/O-problema-N+1.html
---
Performance é uma das minhas preocupações quando estou desenvolvendo uma aplicação. E um dos problemas mais conhecidos relacionado a performance é o problema N+1. Ele acontece quando temos uma consulta que retorna N registros, e para cada um desses registros, fazemos uma nova consulta para buscar informações relacionadas.

Por exemplo, imagine que temos um sistema de blog, onde temos uma tabela de posts e uma tabela de comentários. Se quisermos buscar todos os posts e seus comentários, podemos fazer uma consulta para buscar os posts, e para cada post, fazer uma nova consulta para buscar os comentários relacionados. Isso pode resultar em N+1 consultas, onde N é o número de posts.

Imagine que temos 100 posts, isso significa que teremos 1 consulta para buscar os posts e 100 consultas para buscar os comentários, totalizando 101 consultas. Isso pode ser um grande problema de performance, especialmente se o número de posts for muito grande.

Exemplo:
```sql
SELECT * FROM posts; -- 1 consulta para buscar os posts
SELECT * FROM comments WHERE post_id = 1; -- 1 consulta para cada post
SELECT * FROM comments WHERE post_id = 2;
...
SELECT * FROM comments WHERE post_id = 100;
```

Podemos resolver esse problema com o IN, fazendo uma única consulta para buscar os comentários relacionados a todos os posts de uma vez.
```sql
SELECT * FROM posts; -- 1 consulta para buscar os posts
SELECT * FROM comments WHERE post_id IN (1, 2, ..., 100); -- 1 consulta para buscar os comentários relacionados a todos os posts
```

Dessa forma, reduzimos o número de consultas de N+1 para apenas 2, melhorando significativamente a performance da aplicação.

No Rails, por exemplo, podemos usar o método `includes` para evitar o problema N+1. Ele faz um eager loading dos registros relacionados, evitando consultas adicionais para cada registro.
```ruby
posts = Post.includes(:comments).all
```

O que o include faz por debaixo dos panos:
```sql
SELECT * FROM posts; -- 1 consulta para buscar os posts
SELECT * FROM comments WHERE post_id IN (1, 2, ..., 100); --
```

Uma ferramenta que usso para indentificar o problema N+1 é o Bullet, uma gem para Rails que detecta consultas N+1 e outras consultas ineficientes, alertando o desenvolvedor para que ele possa corrigir o problema. 

Em resumo, o problema N+1 é um problema de performance no backend. Mas que pode ser evitado com boas práticas de desenvolvimento e ferramentas de detecção. 
