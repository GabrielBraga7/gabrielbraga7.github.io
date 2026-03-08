+++
title = 'Tutorial 2 - Setup KW e build do kernel'
date = 2026-03-08T20:10:30-03:00
draft = false
+++

{{< back >}}
## 1. Considerações iniciais

Achei muito interessante o fato de estarmos usando um *toolkit* em linha de comando para automatizarmos/simplificarmos comando(s), além de ser muito útil para o desenvolvimento, ajuda demais pessoas como eu que estão entrando nesse universo de desenvolvimento do kernel agora. Apesar de já ter tido experiências similares, com scripts simples de automação de comandos repetitivos ou a criação de alguns apps CLI, não chega perto do tamanho do kw.

Além disso, é incrível notar que, apesar de termos a possibilidade de utilizarmos e desenvolvermos ferramentas auxiliares como o kw, apenas a utilização de makefiles, variáveis/flags e scripts são suficientes para um desenvolvedor do kernel, algo que é enorme e complexo.

## 2. Instalação/configuração do kw e clone do IIO

A instalação ocorreu sem problemas, apenas tive a curiosidade de pesquisar mais a fundo sobre a utilização de branches stable/unstable, e vi que é basicamente uma medida organizacional para, enquanto uma nova versão com novas features é desenvolvida, a atual pode continuar a ser usada, testada e corrigida separadamente.

Ao clonar o repo. do IIO, fiquei interessado e pesquisei sobre as branches principais do kernel, e achei muito interessante de saber que a branch mantida pelo Linus Torvalds é onde as novas features e desenvolvimento acontece, enquanto a stable é basicamente onde vão parar as mudanças quando ocorre uma release do kernel.

Saber que o kw gerencia e fornece uma interface TUI para o gerenciamento de configurações, mas que é basicamente uma interface para alterar os dotfiles e agregar alguns comandos make/qemu-related foi muito útil para meu entendimento sobre a ferramenta.

## 3. Configurações para compilação do kernel

Para utilizar o comando "kw ssh --get '~/vm_mod_list'", foi necessário instalar o rync na VM - a pasta compartilhada entre VM e host poderia ser usada, mas optei por analisar o erro e tentar corrigi-lo.

Na etapa de gerar o arquivo .config, tive que pesquisar para entender o comando ```
make -C "$IIO_TREE" olddefconfig```, e descobri que a keyword "olddefconfig" serve para, quando já temos um .config criado, atualizar as configurações com as padrões.

A TUI aberta pelo comando ```make -C "$IIO_TREE" nconfig``` lembra muito outras que já utilizei, como o htop e o Nano.

## 4. Compilação do kernel

Até a instalação dos módulos na VM, tudo ocorreu bem. Tive curiosidade sobre o que o comando de  ```kw deploy --modules``` faz, e vi, através do tutorial sem o kw do flusp (https://flusp.ime.usp.br/kernel/build-linux-for-arm/), que ele dá um mount no rootfs da VM com ela desligada, e instala os módulos compilados na VM. A compilação levou 11min., sendo que a primeira tentativa levou 5min. mas deu um erro não mapeado.

## Considerações finais

Muito útil e prático ter seguido o tutorial com o kw, muito em linha e seguindo alguns ótimos padrões de interface de aplicações, com alguns lembrando muito o git. Por fim, um levantamento interessante que surgiu desse tutorial foi de se existe um estudo mais detalhado sobre o impacto, em termos de tempo e qualidade da compilação, da cross-compilation vs a nativa em diferentes cenários.