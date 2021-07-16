---
layout: post
title:  O que fazer quando o brew não consegue baixar nada?
tags:
- brew
- homebrew
- macos
---

<img src="https://brew.sh/assets/img/homebrew-social-card.png" alt="Girl in a jacket" width="750" height="350"> 
<br>
<br>

## Breve historia

Recentemente eu adquiri um macos, simples, porém eficaz para o que eu preciso.
O modelo dele é de 2012, veio com o catalina, porém devido a alguns problemas de gerenciamento de temperaturas tive que migrar para o mojave e fazer umas configurações muitos loucas pra funcionar como deveria, prometo contar em outro post.

Porém, enquanto fazia as configurações necessarias para começar meu trabalho esbarrei num problema, o brew não baixava nada.
<br>
<br>

<center><img src="https://media.giphy.com/media/nVTa8D8zJUc2A/giphy.gif"></center>
<br>
<br>

## O que é o brew?

O brew, ou homebrew, é um gerenciador de pacotes, basicamente como o [proprio site diz](https://brew.sh/), **"O Homebrew instala as coisas que você precisa que a Apple (ou o seu sistema Linux) não forneceram para você."**

Pra quem saiu do mundo linux, ele é o equivalente ao [APT](http://www.bosontreinamentos.com.br/linux/sistema-de-gerenciamento-de-pacotes-apt-no-linux/) para os debian based, ou [pacman](https://wiki.archlinux.org/title/Pacman_(Portugu%C3%AAs)) para o Arch.

Resumindo ainda mais, você consegue instalar um monte de coisas usando comandos no terminal.
<br>
<br>

## Como ele funciona.

> Aviso: Essa seção está totalmente baseada no meu pequeno periodo de experiencia usando a ferramenta, sendo assim algumas coisas com certeza estarão erradas e estou totalmente ciente desse fato e disposto a fazer correções a medida que tiver mais conhecimento a respeito do funcionamento do brew.

O homebrew realiza suas instalações compilando o codigo fonte das ferramentas unix e até open source, existe também o Cask que extende as funcionalidades do brew e instala binarios, que para os usuarios de macos são os aplicativos que você precisa arrastar para a sua pasta de aplicativos.

O brew busca esses aplicativos/ferramentas do seu repositorio base, o [homebrew/core](https://github.com/Homebrew/homebrew-core), por isso é bem comum que ao tentar instalar alguma coisa utilizando o brew ele faça uma atualização primeiro, por que ele está fazer um "git pull" no repositorio base.

Os codigos fonte, ou pelo menos as referencias para a compilação deles ficam nesse repositorio, ou ainda no [homebrew-cask](https://github.com/Homebrew/homebrew-core) e assim por diante.
<br>
<br>

## Certo, mas ele não baixa nada e ai?

Em primeiro lugar o ideal seria realizar uma nova instalação, desistale o brew e comece a instalação do zero.

Primeiro desinstale:

`/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/uninstall.sh)"`

Depois instale:

`/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`

<br>

---

Você também pode usar o doctor do brew, ele indica alguns possiveis problemas que o brew está enfrentado e pode ser que apenas isso já resolva os problemas(geralmente quando ele acha um problema ele também já te da uma solução).

`brew doctor`

---
<br>

Se o problema persistir, vamos para uma solução que resolveu o meu problema.
Em primeiro lugar, como eu disse antes o brew utiliza um repositorio para verificar/baixar os pacotes solicitados, esse repositorio é opensource e fica no github.

Sabendo disso, agora você precisa saber onde o brew instalou o repositorio base na sua maquina. De acordo com a [documentação](https://docs.brew.sh/Installation) do proprio brew ele prefere instalar-se no `/usr/local` para macos com processadores da intel, `/opt/homebrew` para processadores da Apple e `/home/linuxbrew/.linuxbrew` no linux.

Ok, contudo existe um lugar mais profundo onde o brew acessa para pegar a referencia dos pacotes e ele fica em: `/usr/local/Homebrew/Library/Taps/homebrew/`. Aqui ficam as referencias para os pacotes que o brew suporta, divididos em repositorios, o homebrew-core(base do brew ou formulae), homebrew-cask(onde ficam os binarios).

No meu caso, tive problema com o homebrew-core e por isso a solução que encontrei foi clonar o repositorio novamente, fazendo:

1. Deletando o `homebrew-core`. Na pasta `/usr/local/Homebrew/Library/Taps/homebrew/` deletei o repositorio `homebrew-core`:

   `rm -rf homebrew-core`

2. Clonando novamente o `homebrew-core`

   `git clone https://github.com/Homebrew/homebrew-core.git`

<br>
Após isso, tudo deve funcionar corretamente. Lembranod que você pode usar o mesmo procedimento para os outros pacotes(ex: homebrew-cask).
<br>
<br>

<center><img src="https://media.giphy.com/media/xUA7bijsxNm6iMh39e/giphy.gif" width="50%" height="50%"></center>
