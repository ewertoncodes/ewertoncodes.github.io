---
layout: post
title: "Como eu penso e escrevo meus testes"
date: 2026-03-27 11:40:00 -0300
categories: backend
tags: [ruby, testes, rspec]
---

![Como eu penso e escrevo meus testes](/images/xfb6xdd39lglaxle9ns9.png)

Testes são uma parte fundamental do desenvolvimento de software. Eles nos ajudam a garantir que nosso código funcione como esperado e que possamos fazer alterações sem quebrar funcionalidades existentes.

Antes de criar uma aplicação, eu gosto de pensar em como vou testá-la. Isso me ajuda a projetar meu código de forma que ele seja mais fácil de testar.

Piramide de testes é uma forma de organizar os testes em uma aplicação. Ela é dividida em três camadas.
Na base temos os testes unitários, no meio os testes de integração e no topo os testes de ponta a ponta.

![Piramide de testes](/images/piramide-testes.svg)

O próximo conceito de como escrever testes é o TDD (Test-Driven Development). 
No TDD, escrevemos o teste antes de escrever o código. O fluxo é o seguinte:

1. Escreva um teste que falhe.
2. Escreva o código mínimo necessário para que o teste passe.
3. Refatore o código para que ele fique mais limpo e organizado.
4. Repita o processo.

Anatomia de um teste. Eu divido em 3 partes, setup, ação e verificação.

{% highlight ruby %}
# setup
let(:user) { User.new(name: "Ewerton") }

# ação
user.save!

# verificação
expect(user).to be_persisted
{% endhighlight %}

Pensando em casos de uso, eu gosto de pensar em como o usuário vai interagir com o sistema. 
O BDD (Behavior-Driven Development) é uma forma de escrever testes que foca no comportamento do sistema.
Eu usei bastante na minha carreira uma gem para testes chamada rspec. O rspec é uma biblioteca de testes para Ruby que implementa o BDD. Aqui eu penso em 3 palavras, Given, When e Then.

1. Given (Dado que): Eu tenho uma calculadora e os números 5 e 10.
2. When (Quando): Eu executo a operação de soma.
3. Then (Então): O resultado deve ser igual a 15.

{% highlight ruby %}
RSpec.describe "Calculator" do
  describe "addition" do
    let(:calculator) { Calculator.new }
    
    context "when adding two positive numbers" do
      it "returns the sum of the two numbers" do
        result = calculator.add(5, 10)
        expect(result).to eq(15)
      end
    end
  end
end

class Calculator
  def add(a, b)
    a + b
  end
end
{% endhighlight %}

Esse exemplo é bem simples, mas ele ilustra bem o conceito de BDD. Isso é um pouco de como penso e como escrevo testes. Mas esses conceitos são um mundo a ser explorado. Não cabe em um único artigo e pretendo escrever mais sobre testes em breve. 
