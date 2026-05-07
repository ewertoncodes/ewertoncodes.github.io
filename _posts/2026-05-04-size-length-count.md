---
layout: post
title: 'Size, Length e Count: Qual a diferença?'
date: 2026-05-04 11:40:00.000000000 -03:00
categories: backend ruby
tags:
  - ruby
  - performance
  - arrays
  - strings
  - enumerables
  - ruby-methods
canonical_url: https://ewertoncodes.github.io/backend/2026/05/04/size-length-count.html
---
![Size, Length e Count: Qual a diferença?](https://media2.dev.to/dynamic/image/width=1000,height=420,fit=cover,gravity=auto,format=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fy1w4rtghzi8wrwax5il9.png)

Ruby é uma linguagem muito expressiva e que nos dá a liberdade de escrever código de várias formas. Por exemplo, se eu quiser contar quantos elementos tem numa lista, eu posso usar pelo menos 3 métodos diferentes para isso.

# No Ruby

O `length`, que é um método usado em arrays, strings e hashes, retorna o número de elementos que o objeto tem. 

```ruby
  array = [1, 2, 3, 4, 5]
  puts array.length # => 5

  string = "Hello, World!"
  puts string.length # => 13
```

O método `size` é basicamente a mesma coisa que o `length`. Ele é um alias do método `length`.

```ruby
  array = [1, 2, 3, 4, 5]
  puts array.size # => 5

  string = "Hello, World!"
  puts string.size # => 13
```

O `count` já é um pouco diferente. Além de contar o total, você pode passar um argumento ou um bloco para filtrar o que quer contar.

```ruby
  numeros = [1, 2, 3, 4, 5, 6]
  
  # Quantos são pares?
  numeros.count(&:even?) # => 3
  
  # Quantas vezes aparece o número 2?
  numeros.count(2) # => 1
```

---

# No Rails

E como funciona no ActiveRecord? Quando estamos trabalhando com ActiveRecord, a escolha entre esses métodos impacta diretamente a performance, já que cada um fala com o banco de dados de um jeito diferente.

O `count` quando você quer garantir que a contagem será feita diretamente no banco de dados.
Sempre que ele for chamado ele vai executar uma consulta SQL para contar os registros.


```ruby
ActiveRecord::Base.logger = Logger.new(STDOUT)
users = User.all
users.count # SELECT COUNT(*) FROM users => 1000
users.count # SELECT COUNT(*) FROM users => 1000

```

O `length` quando é chamado pela primeira vez vai executar uma query no banco de dados para contar os registros, e nas chamadas seguintes ele vai usar o valor que foi armazenado na memória.

```ruby
ActiveRecord::Base.logger = Logger.new(STDOUT)
users = User.all
# O length carrega todos os registros para a memória
users.length # SELECT "users".* FROM "users" => 1000 registros transformados em objetos
users.length # 1000 (agora usa o que já está na memória)


```

Quando usamos o `size` ele consegue saber alternar entre o `count` e o `length`. Ele verifica se os registros foram carregados previamente, se sim ele usa o length, se não ele usa o count.

```ruby
users = User.all
users.size # SELECT COUNT(*) FROM "users" (Ele viu que não estava carregado e foi direto pro banco)

users.load # Forçamos o carregamento dos registros
users.size # 1000 (Ele percebeu que já carregou e contou direto da memória)

```

Entender o comportamento real desses métodos é fundamental para evitar desperdício de recursos. Escolher o método certo para cada cenário é o que garante uma aplicação funcione de forma correta. Nos próximos posts pretendo mostrar outros métodos do Rails que causam confusão mas que são muito importantes para o dia a dia do desenvolvedor Rails.

# Referências

count: [https://apidock.com/rails/ActiveRecord/Calculations/count](https://apidock.com/rails/ActiveRecord/Calculations/count)

length: [https://docs.ruby-lang.org/en/master/Array.html#method-i-length](https://docs.ruby-lang.org/en/master/Array.html#method-i-length)

size: [https://apidock.com/rails/ActiveRecord/Associations/CollectionProxy/size](https://apidock.com/rails/ActiveRecord/Associations/CollectionProxy/size)

