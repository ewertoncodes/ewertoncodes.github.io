---
layout: post
title:  "Entendendo o JSON Web Token (JWT)"
date:   2025-12-24 00:17:30 -0300
categories: backend
---


Em algum momento, ao criar uma aplicação web, precisamos desenvolver uma solução de autenticação para o sistema. Existem várias estratégias para isso, como autenticação por senha,
tokens, OAuth, entre outras.

Nesse post, vou falar apenas sobre o JWT (JSON Web Token), uma abordagem bastante utilizada no desenvolvimento de APIs.

O JWT é um padrão aberto usado para transmitir informações de forma compacta e segura entre  partes que usa JSON. Uma de suas características é ser stateless, ou seja, o servidor não precisa armazenar nenhuma informação sobre a sessão do usuário. Todas as informações nescessárias estão contidas no próprio token.

Essa abordagem é especialmente útil em aplicações onde o backend e o frontend estão separados, como em aplicações mobile e SPAs (Single Page Applications).

## Anatomia do JWT

O JWT é composto por três partes, header, payload e assinatura separados por um ponto (.).]

```bash
xxxxx.yyyyy.zzzzz
```

### Header

O Header tem duas partes. O tipo do token, que é jwt e o algoritimo de assinatura utilizado. Tais como HMAC SHA256 ou RSA. 

Exemplo:

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

Em seguida esse Json é codificado para Base64Url para formar a primeira parte do JWT.

### Payload

A segunda parte do jwt é o payload, um objeto json com as informações da entidade tratada, geralmente é um user.

```json
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true
}
```

### Signature

A terceira parte do jwt é a assinatura.  Ela é formada pelo header  mais o payload  encodados em Base64Url e um secret. A assinatura é a parte mais importante  do jwt pois é ela que garante  a integridade do nosso token.

{% highlight js %}
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret)
{% endhighlight %}

Colocando tudo junto o jwt ficaria assim:

{% highlight js %}
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWUsImlhdCI6MTUxNjIzOTAyMn0.KMUFsIDTnFmyG3nMiGM6H9FNFUROf3wh7SmqJp-QV30
{% endhighlight %}


### Como validamos a Assinatura?

![image](/images/jwt.png)


Quando o servidor recebe o token, ele inicia um processo de verificação para garantir que aquelas informações são legítimas. Em vez de tentar "descriptografar" a assinatura, o servidor opta por **reconstruí-la**. Ele separa o Header e o Payload enviados pelo usuário e, utilizando a sua própria **Secret** (chave secreta) guardada no ambiente seguro do backend, aplica novamente o algoritmo de hash.

O resultado desse cálculo gera uma nova assinatura que é então comparada com a assinatura que veio originalmente no token. Se ambas forem exatamente iguais, o servidor tem a prova matemática de que o token é íntegro e pode confiar nos dados contidos ali.

JavaScript
{% highlight js %}
const novaAssinatura = hash(`${headerSent}.${payloadSent}`, 'sua_secret_aqui');

if (novaAssinatura === signatureSent) {
  // O token é íntegro e válido!
}
{% endhighlight %}

Se uma pessoa mal-intencionada interceptar um token JWT e tentar alterar o role no payload — por exemplo, mudando de user para admin — qualquer modificação em um único caractere fará com que a assinatura original deixe de ser válida, e o token será rejeitado pelo servidor.

Para que o servidor aceitasse essa alteração, o atacante precisaria gerar uma nova assinatura que batesse com o novo conteúdo. No entanto, como a assinatura depende obrigatoriamente da **Secret** para ser gerada, e essa chave só o servidor possui, o invasor não consegue concluir o ataque. Sem a chave secreta, qualquer tentativa de alteração resulta em uma assinatura inválida, e o sistema bloqueia o acesso imediatamente.

O JWT é uma ferramenta poderosa para a autenticação , especialmente por ser **stateless**. Isso permite que sua aplicação escale facilmente, já que o servidor não precisa consultar um banco de dados de sessões a cada requisição.

No entanto, lembre-se o JWT protege a integridade dos dados, mas não a sua privacidade (já que o payload é visível). Portanto, mantenha sua `Secret` bem guardada, nunca armazene informações sensíveis como senhas dentro do token, crie uma secret que não seja fácil de ser quebrada com brute force. Com esses cuidados, o JWT se torna uma solução robusta e eficiente para o controle de acesso da sua API.
