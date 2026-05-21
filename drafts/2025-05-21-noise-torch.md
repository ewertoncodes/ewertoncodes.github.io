---
layout: post
title: Como Sumir com o Barulho do Teclado Mecânico no Ubuntu Usando o NoiseTorch
date: 2026-05-21 00:51:55.000000000 -03:00
categories: ubuntu
tags:
  - ubuntu
  - noisetorch
  - tutorial
canonical_url: https://ewertoncodes.github.io/post/noisetorch
---

Se você trabalha remoto, com certeza já passou por isso: você está em uma reunião super importante, começa a digitar suas notas ou responder um chat, e alguém solta o clássico: "Ewerton, dá uma segurada aí que o seu teclado está parecendo uma metralhadora!".

O barulho de cliques mecânicos ou digitação mais agressiva é o terror das chamadas de vídeo. Embora ferramentas como Google Meet e Zoom tenham seus próprios filtros, eles nem sempre dão conta ou acabam pesando demais no navegador.

Se você usa Linux (como o Ubuntu), existe uma solução open-source, ultra leve e que roda direto na sua máquina (sem mandar seu áudio para nuvens terceiras): o NoiseTorch.

Neste post, vou te mostrar o passo a passo definitivo para instalar, corrigir os bugs clássicos de ícone no Ubuntu e colocar ele para rodar na sua próxima reunião.

O que é o NoiseTorch?
O NoiseTorch cria um microfone virtual no seu sistema. Ele captura o áudio do seu microfone físico, passa esse som por uma biblioteca de inteligência artificial leve (chamada RNNoise) que filtra o que é voz e o que é ruído (teclado, ventilador, cachorro latindo), e entrega um áudio totalmente limpo para o Zoom, Meet, Slack ou Teams.

Passo a Passo da Instalação no Ubuntu
Como o NoiseTorch não está na loja padrão do Ubuntu, nós vamos fazer a instalação pelo terminal. Preparado? Abre o terminal (Ctrl + Alt + T) e vamos nessa:

Passo 1: Instalar as dependências básicas
Primeiro, precisamos garantir que o sistema tem as ferramentas necessárias para baixar e descompactar os arquivos:

```bash
sudo apt update && sudo apt install -y wget tar gtk-2.0
```

Passo 2: Baixar a versão estável e correta
Para evitar links quebrados ou arquivos HTML corrompidos que dão erro no terminal, baixe diretamente a versão estável (v0.12.2) direto para a sua pasta de Downloads:

```bash
cd ~/Downloads && wget https://github.com/noisetorch/NoiseTorch/releases/download/v0.12.2/NoiseTorch_x64_v0.12.2.tgz
```

Passo 3: Extrair para os diretórios locais
Os pacotes do NoiseTorch já vêm organizados na estrutura de pastas que o Linux usa. Vamos descompactar tudo direto na pasta local do seu usuário (~/.local):

```bash
tar -C $HOME -h -xzf NoiseTorch_x64_v0.12.2.tgz
```

(Se quiser limpar a casa, já pode apagar o instalador rodando rm NoiseTorch_x64_v0.12.2.tgz).

Passo 4: Dar permissões de sistema (Essencial!)
O NoiseTorch precisa gerenciar os fluxos de áudio do seu PulseAudio ou PipeWire sem precisar rodar como administrador (root). Para dar essa permissão especial a ele, rode:

```bash
sudo setcap 'CAP_SYS_RESOURCE=+ep' ~/.local/bin/noisetorch
```

Corrigindo o Bug do "Ícone de Engrenagem" no Ubuntu
Se você tentar rodar o NoiseTorch agora, o Ubuntu pode exibir apenas uma engrenagem genérica no dock e não vai deixar você fixar o aplicativo nos favoritos.

Para resolver isso e deixar o atalho com o ícone roxo oficial bonitinho, rode este bloco de comandos para atualizar o cache do GNOME:
```bash

# Corrige o caminho do executável dentro do atalho desktop
sed -i "s|Exec=noisetorch|Exec=$HOME/.local/bin/noisetorch|g" ~/.local/share/applications/noisetorch.desktop

# Garante que o ícone do app vá para a pasta correta de renderização
mkdir -p ~/.local/share/icons/hicolor/apps/
cp ~/.local/share/icons/hicolor/scalable/apps/noisetorch.svg ~/.local/share/icons/hicolor/apps/ 2>/dev/null || true

# Força o Ubuntu a ler as modificações de aplicativos
update-desktop-database ~/.local/share/applications
gtk-update-icon-cache -f ~/.local/share/icons/hicolor
```
Como Usar na Prática
Abra o menu de aplicativos do Ubuntu (tecla Super / Windows) e busque por NoiseTorch.

Abra o app, selecione o seu Microfone Real na lista que aparecer na tela e clique no botão grande "Load NoiseTorch".

![noisetorch interface](/assets/img/noisetorch.png)

Agora, quando você entrar no Google Meet, Zoom ou Slack, vá nas configurações de áudio da chamada e mude o microfone para:

NoiseTorch Virtual Microphone

Como testar se funcionou?
Quer ter certeza de que o teclado sumiu antes de entrar na sala com o cliente ou com o time? Você pode gravar um áudio local de 10 segundos para testar. Rode no terminal:

```bash
arecord -D pulse -f cd -d 10 teste_ruido.wav
```
Nos primeiros 5 segundos: Fale normalmente enquanto digita no teclado.

Nos últimos 5 segundos: Fique em silêncio e apenas digite com força.

Para ouvir o resultado, execute:

```bash
aplay teste_ruido.wav
```
Se tudo deu certo, na metade final do áudio você vai ver que o NoiseTorch simplesmente dropou o som das suas teclas, deixando um silêncio absoluto. Salvação pura para o dia a dia do home office!

E você, já conhecia o NoiseTorch ou sofria configurando o áudio em cada app diferente? Deixa nos comentários se o tutorial funcionou para você!
