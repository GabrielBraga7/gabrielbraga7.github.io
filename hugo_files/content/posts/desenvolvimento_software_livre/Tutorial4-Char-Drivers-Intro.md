+++
title = 'Tutorial 4 - Introdução aos "Character Device Drivers"'
date = 2026-03-24T22:22:00-03:00
draft = false
+++

{{< back >}}
## 1. Considerações iniciais

Sempre tive muita curiosidade para saber exatamente o que significavam alguns arquivos que acabava vendo por vezes em meu sistema, como os vários existentes em ```/dev/*```. Ao tentar dar um cat ou aplicar alguma outra interação simples, não obtinha sucesso - acabei descobrindo depois que não eram "arquivos comuns", mas com este tutorial consegui compreender melhor o que exatamente são esses arquivos especiais.

## 2. Entendendo os conceitos e interagindo com o código

Achei a implementação em C da representação do identificador do dispositivo muito interessante, imaginava que seria algo mais complexo a interação com os dispositivos, porém, a estrutura de manipulação de bits com a struct do C + character device file se mostrou elegantemente simples (na medida do possível). Muito legal!

Apesar de já ter utilizado C, nunca utilizei-o em um contexto de uma aplicação mais profissional, e nunca cheguei perto de algo tão robusto como o kernel linux. De início, tive que relembrar alguns conceitos para entender a sintaxe de algumas partes, como o tipo void* e os string formatters.

Havia ficado confuso com as nomenclaturas usadas, porém cheguei à seguinte conclusão sobre as nomenclaturas e conceitos: há os **character devices**, e dentre eles temos os **character device files**, também chamados de **device node**, **device file**, ou **device special file**. Além disso, é possível termos arquivos comuns administrados pelos character device files, como a imagem do linux.

Outro ponto interessante é o da necessidade de cópia entre os espaços de memória do kernel e do usuário. Apesar de ter entendido superficialmente, fiquei curioso sobre a existência/implementação de restrições de acesso a determinados espaços de memória, como o PAGE_OFFSET é aplicado, na prática, para garantir a segregação dos espaços, e se há alguma alternativa de otimização deste modelo que necessita da cópia de dados entre os espaços.

## 3. Considerações finais

Nos exercícios propostos, obtive um pequeno problema ao tentar realizar o primeiro -> percebi o meu erro de ter adicionado o "print" das informações antes da execução do cdev_add, então guardando o retorno em uma variável, realizando o "print" e retornando logo após resolveu o problema:

	int mapping = cdev_add(s_cdev, dev_id, MINOR_NUMS);
	
	pr_info("Major: %u\n", MAJOR(dev_id));

	pr_info("Minor: %u\n", MINOR(dev_id));

	return mapping;