
+++
title = 'Contribuições para o kernel linux'
date = 2026-04-21T19:50:00-03:00
draft = false
+++

{{< back >}}


## 1. Considerações iniciais

Para realizar a confecção dos PATCHES para o kernel linux, formei um grupo com o [Ellian Carlos](https://elliancarlos.com.br/posts/tag/mac0470/) e o [Ricardo Kojo](https://ricardokojo.github.io/MAC5856-2026/), e escolhemos duas sugestões de contribuições: a deduplicação de código das funções de init de dois devices do iio, que ficam no arquivo drivers/iio/light/veml6030.c::veml6030_hw_init e veml6035_hw_init, e a substituição de chamadas `mutex_lock(&lock)` e `mutex_unlock(&lock)` por `guard(mutex)(&lock)`, a fim de trazer uma segurança maior na utilização de locks. Eu acabei trabalhando mais no PATCH da deduplicação, enquanto o Ellian e o Ricardo trabalharam mais no do mutex, porém com contribuições simultâneas e importantes entre nós, de forma que todos acabaram ajudando em cada etapa de ambos.

## 2. PATCH de deduplicação

Para este PATCH, a estratégia abordada foi de encapsular os valores de configurações diferentes entre as funções de init em uma struct nova, declarar duas variáveis constantes deste tipo com os valores para o veml6030 e o veml6035, criar uma função genérica que aceite este parâmetro e chamar ela no init do 6030 e do 6035, assim eliminando o código duplicado. o gain_sel_size teve que ser incluido pois chamar a macro ARRAY_SIZE para o gain_sel acabou não funcionando devido ao tipo ser ponteiro. Enviamos o patch para o CI do IME, passou nos testes e enviamos o PATCH, com uma resposta amigável indicando possíveis mudanças, como a remoção do caractere 'x' nas declarações e a remoção das funções wrapper. Ainda iremos avaliar, realizar as mudanças e ressubmeter uma v2.

```
struct veml603x_hw_init_config {
	const struct iio_gain_sel_pair *gain_sel;
	int gain_sel_size;
	int gain_max_scale_int;
	int gain_max_scale_nano;
	unsigned int reg_als_conf_value;
};
```

## 3. PATCH do mutex

Apenas revisei e me comuniquei com o Ellian e o Ricardo sobre este PATCH, mas ambos tiveram maior contribuição. Neste, selecionamos o driver /drivers/block/null_blk, que além de não ter uma complexidade alta de código, é uma peça importante. Importante mencionar que foi utilizado o spin_lock e spin_unlock, e os arquivos modificados foram main.c, zoned.c and nullblk.h. Já recebemos uma resposta não muito amigável :P que questionou o motivo de termos atribuído um retorno de função a uma variável, apenas para retorná-la na linha seguinte. Acabamos removendo esta etapa intermediária e reenviamos uma v2, pendente de resposta ainda. 

