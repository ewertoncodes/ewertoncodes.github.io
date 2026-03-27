---
layout: post
title: "Observabilidade com Prometheus e Grafana"
date: 2026-03-26 23:51:00 -0300
categories: devops
tags: [devops, prometheus, grafana, observabilidade, rails, docker]
---

## Observabilidade com Prometheus e Grafana

Recentemente comecei a explorar o mundo da observabilidade, e uma das ferramentas que mais me impressionou foi o Prometheus, junto com o Grafana para visualização.

Antes de tudo vamos iniciar  uma aplicação Rails simples e configurá-la para expor métricas para o Prometheus.

dockerfile rails
```
FROM ruby:3.2
WORKDIR /app
COPY Gemfile* ./
RUN bundle install
COPY . .
EXPOSE 3000
CMD ["bundle", "exec", "puma", "-C", "config/puma.rb
"]
```
docker compose yaml
```version: '3'
services:
  web:
    build: .
    ports:
      - "3000:3000"
    environment:
      - RAILS_ENV=development
    volumes:
      - .:/app
    depends_on:
      - db
  db:
    image: postgres:13
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
      - POSTGRES_DB=app_development
    ports:
      - "5432:5432"
```



