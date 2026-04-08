+++
title = 'Tutorial 7 e 8 - Aprendizado, configuração e implementações novas do iio_simple_dummy'
date = 2026-04-07T22:03:00-03:00
draft = false
+++

{{< back >}}

## 1. Considerações iniciais

Muito interessante estar interagindo com código C produtivo e de uma forma mais direta neste tutorial, consegui aprender alguns usos de sintaxe e de estruturas que não havia sido apresentado, como a atribuição das propriedades de cada elemento da struct ```iio_chan_spec``` da forma ```.<prop> = <value>``` . Porém, confesso que, diferente dos outros tutoriais, o conteúdo foi mais denso e demorado para captar.

## 2. Aprendendo sobre o iio_simple_dummy

Ao ser apresentado aos conceitos que estão contidos na declaração da ```struct iio_chan_spec iio_dummy_channels[]```, uma dúvida que surgiu foi a de o que o valor numérico, quando o .modified não é atribuído, significa. Vou questionar e trago uma resposta :)

Acho importante também ressaltar a declaração do ```.scan_type```, como ele permite o controle fino da tipagem, tamanho e especificações dos dados do buffer. Me deixou curioso para saber como que esse controle é de fato implementado.

Uma sugestão para o tutorial 7 que tenho seria de explicar explicitamente sobre o tipo ```iio_sw_device```, que não ficou 100% claro, apesar de ter entendido de forma superficial seu uso no código.

Por fim, pesquisei sobre o kzalloc vs outras formas de [alocação de memória](https://www.kernel.org/doc/html/latest/core-api/memory-allocation.html) e gostei de descobrir que existem diversas flag's e formas de alocar memória. Até então, só tinha utilizado o malloc, e além disso, não esperava ver o uso de "goto" no código, apesar de entender seu uso neste caso.

## 3. Compilando e testando o iio_simple_dummy

Ao compilar o iio_simple_dummy, por vezes, o comando ```make build``` sozinho, ou até mesmo o utilizado no tutorial ```make M=drivers/iio/dummy``` não funcionam corretamente, sendo necessário utilizar o kw.

Montar o configfs, que é um filesystem virtual que permite manipulação de objetos do kernel, foi uma experiência muito interessante, já que aparentemente ele mexe no seu fstab (onde mapeamos os discos, mount points e suas configs), além de tornar extremamente simples criar um novo objeto, com um mkdir.

Pesquisei também sobre as diferenças entre o sysfs (/sys) e o configfs, e descobri que o sysfs serve mais a um propósito de visualização de device, drivers e estruturas do kernel. Com essas pesquisas, acabei descobrindo que o configfs fica em /sys/kernel/config, o que me leva a questionar o porquê montamos ele novamente? :P (apesar do questionamento, gosto do uso do mount como potencial levantador de curiosidades).

# 4. Modificando o código, implementando e testando (novamente)

Na seção de adicionar canais para um compasso de 3 eixos, ao seguir o tutorial, percebi que o arquivo está levemente diferente da versão utilizada no tutorial, sendo que as modificações devem ser adaptadas, tanto em questões de lugar (ao invés de mudar o iio_dummy_read_raw, por exemplo, deve-se mudar o \_\_iio_dummy_read_raw). Apesar disso, as adaptações são simples.

## 5. Considerações finais

Certamente foi uma aventura muito interessante a de modificar um código mais complexo em C, compilá-lo e carregá-lo. Apesar do salto de complexidade em termos de conceito/familiaridade, no meu caso, achei muito importante e proveitoso esta etapa para a realização do futuro patch.