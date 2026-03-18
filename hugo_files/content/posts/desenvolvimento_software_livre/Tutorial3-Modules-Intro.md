+++
title = 'Tutorial 3 - Introdução aos módulos do linux'
date = 2026-03-17T21:13:30-03:00
draft = false
+++

{{< back >}}
## 1. Considerações iniciais

Logo de início, achei muito interessante a "interface" utilizada para a criação e comunicação com os módulos, como a definição da licença do módulo como ```MODULE_LICENSE("GPL")``` e a definição de dependências entre módulos como ```EXPORT_SYMBOL_NS_GPL(<function-export>, "<name>")```, por ser uma maneira simples mas efetiva de implementar estes conceitos.
## 2. Criando e configurando os módulos

Nesta parte, achei que poderia existir (não consegui descobrir se existe) uma ferramenta para adicionar a configuração e já organizar o arquivo seguindo as melhores práticas - além disso, caso o arquivo seja rígido em relação à formatação para manter a integridade das informações (não testei adicionar os módulos com uma formatação diferente), seria muito interessante ter algo do tipo.

Fiquei apenas com a dúvida/curiosidade de saber o porquê adicionamos o prefixo obj- para o Makefile, provavelmente deve ser um padrão de nomenclatura.

Percebi que, no tutorial, alguns recursos do kw utilizados previamente não foram utilizados aqui, como o comando ```make -C "$IIO_TREE" menuconfig```ter sido usado ao invés do ```kw build --menu```. Preferi a interface do segundo então acabei utilizando-o. Além deste, o ```kw deploy --modules```não foi utilizado também, e tive alguns problemas de permissão ao usar o guestmount (consegui resolver com chmod e setfacl), além do problema ao tentar desmontar pelo comando do tutorial, já que a variável VM_MOUNT_POINT não foi definida.

Sempre tive curiosidade sobre os logs do sistema/kernel, gostei bastante de saber sobre o ```dmesg```.

Por fim, outras coisas que me atrapalharam um pouco foram:
- O fato de, na hora de eu salvar o .config, ter selecionado a opção de salvar com outro nome na TUI (o que acabou fazendo com que os módulos não estivessem disponíveis no primeiro build);
- O IP da VM acaba sendo trocado periodicamente, preciso encontrar uma maneira de fixá-lo. Para essa questão e algumas outras, criei alguns scripts .sh na raíz do projeto (get_rootfs_img_disk.sh, get_vm_ip.sh, connect.sh) para ajudar com comandos que utilizei bastante.

## 3. Considerações finais

Ao realizar os exercícios propostos, o primeiro deixou minha VM com o terminal "travado", provavelmente devido à coisas importantes terem sido deixadas como módulos (ou eu fiz alguma besteira nessa parte), para reverter bastou eu refazer o .config, recompilar tudo, montar o disco da VM e passar os módulos para lá. Muito interessante esse tutorial, curioso para ver mais sobre o desenvolvimento e arquitetura dos módulos do kernel!