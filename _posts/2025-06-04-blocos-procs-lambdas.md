---
layout: post
title:  "Blocos, Procs e Lambdas no Ruby"
date:   2025-06-04 00:18:20 -0300
categories: backend ruby
---


Ruby é famoso por sua sintaxe elegante e recursos poderosos, entre eles os **Blocos**, **Procs** e **Lambdas**. Se você está começando com Ruby ou quer reforçar o entendimento, este post vai ajudar a esclarecer o que são esses recursos, como funcionam, e quando usá-las.

## Blocos
Um **bloco** é um pedaço de código que pode ser passado para um método. Um exemplo bastante comum é seu uso em iterações, como no método `each`. Podemos usar blocos tanto inline, com os caracteres `{}`, quanto em múltiplas linhas, com `do .. end`.

```ruby
array = [1, 2, 3]

array.each{ |item| p item }

array.each do |item|
  p item
end
```

## Procs

**Procs** são objetos que encapsulam blocos de código. Eles são flexíveis quanto à passagem de argumentos:

- Se passarmos mais argumentos do que o esperado, os extras são ignorados.
    
- Se não passarmos o argumento esperado, a execução continua normalmente.


```ruby
my_proc = Proc.new { puts 'my proc' } # ou proc {puts 'my proc'}
my_proc.call

my proc
=> nil


other_proc = Proc.new { |x| "x is #{x}"  }
other_proc.call(2)
=> "x is 2"
other_proc.call
=> "x is "

other_proc.call(5,2)
=> "x is 5"

```


## Lambdas

Se nenhum argumento for passado, um erro será exibido.
**Lambdas** são muito parecidas com Procs, mas são **estritas** em relação aos argumentos. Se você não passar o número exato de argumentos, um erro será gerado. 


```ruby
my_lambda = lambda { |x| "x is #{x}" }
my_lambda.call()

 `block in <top (required)>': wrong number of arguments (given 0, expected 1) (ArgumentError)`
```
## Conclusão

Em resumo, blocos, Procs e lambdas oferecem formas poderosas e flexíveis de lidar com trechos de código em Ruby. Os blocos são ideais para usos rápidos, como em iterações, enquanto Procs permitem reutilizar lógica de forma mais solta, sem tanta preocupação com os argumentos. Já as lambdas se comportam de maneira mais próxima às funções tradicionais, garantindo que os parâmetros sejam respeitados com rigor.

Entender essas diferenças ajuda não só a escrever um código mais limpo e expressivo, mas também a fazer escolhas mais conscientes em projetos reais. Com o tempo e a prática, você vai perceber que esses conceitos são fundamentais para aproveitar todo o potencial da linguagem Ruby.
