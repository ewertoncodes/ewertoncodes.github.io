---
layout: post
title: "Compartilhando Comportamentos usando Herança no Ruby"
date: 2026-03-17 11:40:00 -0300
categories: backend
---
## Compartilhando Comportamentos usando Herança no Ruby

Um dos pilares da programação orientada a objetos é a herança, que é um conceito que permite que uma classe herde atributos e métodos de outra classe. No Ruby, a herança é feita usando o operador `<`.  

Vamos criar uma classe base simples:

{% highlight ruby %}
class Character
  def attack
    "Attack"
  end
end
{% endhighlight %}

### O uso básico do `super`

O `super` é a palavra-chave usada para invocar um método da classe pai a partir de uma classe filha. Veja o exemplo de um **Guerreiro (Warrior)** que herda de `Character`:

{% highlight ruby %}
class Warrior < Character
  def attack
    super + " com espada"
  end
end

Warrior.new.attack
#=> "Attack com espada"
{% endhighlight %}

Neste caso, chamar `super` funciona perfeitamente. A classe `Warrior` herda os métodos de `Character` e sobrescreve o método `attack`. A chamada para `super` executa o `attack` da classe pai.

### A diferença entre `super` e `super()`

A confusão começa quando o método da classe filha exige argumentos que a classe pai não espera (ou vice-versa).

Por padrão, **chamar apenas `super`** invoca o método da classe pai repassando de forma implícita **todos os mesmos argumentos** que foram entregues ao método da classe filha. 

Se tentarmos criar um **Mago (Mage)** cujo ataque recebe o nome de uma magia (`spell`), o uso apenas de `super` causará um erro:

{% highlight ruby %}
class Mage < Character
  # O método filho espera 1 argumento (spell)
  def attack(spell)
    super # Isso automaticamente tentará passar o 'spell' para o método pai!
  end
end

Mage.new.attack("Fireball")
#=> "ArgumentError: wrong number of arguments (given 1, expected 0)"
{% endhighlight %}
O erro `ArgumentError` ocorre porque o argumento `spell` (no caso, `"Fireball"`) foi repassado automaticamente por debaixo dos panos para o método `Character#attack`, mas a classe `Character` estava esperando **zero argumentos**.

Para resolver isso, precisamos usar **`super()`** com os parênteses:

{% highlight ruby %}
class Mage < Character
  def attack(spell)
    super() + " com magia #{spell}"
  end
end

Mage.new.attack("Fireball")
#=> "Attack com magia Fireball"
{% endhighlight %}

**Resumo:**
- **`super`**: Invoca o método da classe pai repassando exatamente os **mesmos argumentos** que o método atual recebeu.
- **`super()`**: Invoca o método da classe pai explicitamente **sem nenhum argumento**, não importa quantos argumentos o método atual tenha recebido.

Como sempre, ser explícito no seu código é uma boa prática e evita comportamentos inesperados ao sobrescrever métodos!
