+++
title = 'Tutorial 1 - Setup QEMU e libvirt'
date = 2026-02-26T23:17:30-03:00
draft = false
+++
## 1. Considerações iniciais

De início, pude observar que minha experiência prévia com Linux me ajudou na resolução de alguns problemas/erros que obtive durante o tutorial (valeu a pena ser curioso :D ). Achei muito interessante estarmos usando um software (QEMU) que, além de ter a licença GPL, possui como interface principal a linha de comando, já que todas as outras ferramentas que usei de virtualização (VirtualBox e HyperV, por exemplo) possuem interface gráfica e configurações diferentes.

## 2. Instalação de dependências

Pela minha distro (Linux Mint) ser uma derivada do Ubuntu, que por sua vez é derivada do Debian, não obtive problemas relacionados à instalação ou similares.

## 3. Configurações de diretório/script  de ambiente

Nesta etapa, mesmo adicionando meu usuário ao grupo do libvirt-qemu, não obtive sucesso ao tentar realizar modificações com ele. A solução foi utilizar o comando setfacl para adicionar permissões adicionais para meu usuário, especificamente.

Ao realizar o download da imagem do debian, percebi que o retorno, ao realizar a requisição para o link da imagem do sistema, era de 404. A fim de obter uma imagem, apenas retornei para o caminho que contém as imagens (https://cdimage.debian.org/cdimage/cloud/bookworm/daily) e obtive a https://cdimage.debian.org/cdimage/cloud/bookworm/daily/20260223-2397/debian-12-nocloud-arm64-daily-20260223-2397.qcow2, que segue as mesmas especificações da utilizada no tutorial.

Despertou meu interesse a extensão da imagem, e vi que ela possui recursos que permitem uma maior economia de espaço, uso de snapshots e encriptação/compactação, sendo ótima para nosso propósito.

## 4. Comandos relacionados à imagem

Alguns comandos, como o virt-filesystems, não rodavam sem privilégios elevados. Ao configurar o comando para obter logs detalhados, pude observar que, por algum motivo, este comando tenta acessar o arquivo "/boot/vmlinuz-6.8.0-90-generic", que seria o kernel do meu sistema. Rodar os comandos com sudo resolveu este problema.

Apesar de já ter sido introduzido ao rootfs e à nomenclatura de /dev/sdaX anteriormente, um ponto interessante foi a menção ao initrd, algo que não sabia exatamente o conceito, e ao pesquisar descobri que ele carrega um sistema de arquivos temporário em memória, carregando os principais módulos e preparando o mount do sistema de arquivos raíz. (aprendizados importantes e divertidos :D)

## 5. Subindo a VM e configurando-a

Logo de início, o libvirtd me lembrou bastante a utilização de docker para conteinerização, mesmo que ambos sejam utilizados para coisas distintas. Tive que utilizar alguns comandos do virsh, como destroy e undefine para resolver alguns comandos errados e recriar a VM :P

### 5.1 Configuração SSH

Por algum motivo, o sistema estava sem o openssh-server instalado, tive que dar um apt-get update e instalá-lo. Após, consegui seguir com a configuração (já usei e configurei servidores e acessos SSH, então foi bem familiar essa parte). Além disso, criei um script de connect na pasta raíz do projeto-tutorial para não esquecer o IP da VM (obtido através do output do comando "virsh net-dhcp-leases default").

## 5.2 Configuração de pasta compartilhada entre VM <-> host

Um problema que pude perceber no tutorial é o nome utilizado no comando "sudo EDITOR=vim; sudo virsh edit iio-arm64" - o nome iio-arm64 vai variar dependendo do nome atribuído à VM (que no caso do tutorial, era apenas arm64). Além disso, o pacote "virtiofsd" precisou ser instalado para que eu conseguisse completar esta configuração.

## Considerações finais

No geral, foi muito proveitosa e um tanto divertido seguir o tutorial e pesquisar conceitos que eu não eram muito familiares, além de conseguir por em prática alguns conhecimentos na resolução dos problemas que foram surgindo. Empolgado para os próximos passos!