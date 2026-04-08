+++
title = 'Tutorial 5 e 6 - Setup do envio de patchs por e-mail utilizando o kw e git send-email'
date = 2026-04-03T18:32:00-03:00
draft = false
+++

{{< back >}}
## 1. Considerações iniciais

Apesar de já ter trabalhado com diversas plataformas para Git (Github, Azure Devops, etc.) e de já ter subido e gerenciado serviços git bem simples em servidores internos, nunca chegou ao meu conhecimento que a forma utilizada para abrir *merge requests* para o kernel linux seria via e-mail. Pelo tamanho, natureza, importância e complexidade do projeto, eu esperava uma sofisticação maior de ferramenta, mesmo que o processo em si seja sólido e robusto. Ao pesquisar sobre, vi que alguns dos principais motivos é a escalabilidade, já que o e-mail é rápido, acessível e bom para ser utilizado em automações [¹](https://lwn.net/Articles/702177/). Uma solução simples e que funciona :)

## 2. Configurando o git para envio dos emails

Ao realizar a configuração e alterar o autor/email, percebi que, ao testar o ```git send-email``` em um repositório que já havia realizado commits no passado, o e-mail utilizado para envio é diferente do e-mail do autor dos commits, levantando a questão se não deveria ter uma restrição ou controle neste tipo de divergências. Além disso, vale destacar alguns pontos interessantes de comentar:
- O uso do ```--dry-run``` é muito útil, provavelmente muito utilizado em scripts de automação;
- Sobre o esquema de versionamento de patches, apesar de ser parece simples e funcional, parece haver espaço para melhorias com a introdução de ferramentas;
- A versão do git é adicionada no corpo da mensagem de e-mail, o que também pode ser uma informação útil para scripts de automação;
- Muito interessante que, assim como quando executamos um ```git commit``` e recebemos um resumo das modificações (arquivos, linhas modificadas, etc) que foram realizadas, o corpo do e-mail também a contém, como:
```
test.txt | 1 +  
 emailproxy.config           | 6 +++---  
 2 files changed, 4 insertions(+), 3 deletions(-)  
 create mode 100644 test.txt
```

Uma dica muito interessante foi a da atribuição da variável de ambiente VISUAL/EDITOR, muito útil para manter uma consistência *system-wide*. A VISUAL é utilizada com prioridade, e caso o terminal não suporte a primeira opção, a segunda é utilizada (geralmente editores mais simples são colocados aqui)[²](https://unix.stackexchange.com/questions/4859/visual-vs-editor-what-s-the-difference) .

## 3. Rodando o proxy de email

O setup ser feito com docker é muito bom, uma vez que reduz drasticamente a chance de problemas ocasionados pelo ambiente, além de "empacotar" lógicas de configurações, dependências e run, ideal para o nosso caso aqui. Curiosidade: quem será que criou a aplicação dentro da google relacionadas ao client_id e client_secret utilizados na configuração??

O setup ocorreu sem muitos problemas, apenas tive um problema relacionado à uma configuração do cliente git, pois caso eu não deixasse a opção smtpEncryption vazia (consequentemente desativando a transformação da conexão para TLS), o proxy de emails me retornava: ```Client attempted to begin STARTTLS - please either disable STARTTLS in your client when using the proxy, or set `local_starttls`and provide a `local_certificate_path` and `local_key_path` ```.

## 4. Considerações finais

Gostei muito dos métodos, configurações e aprendizados de ambos os tutoriais, parabéns aos envolvidos :)
A única alteração que eu sugeriria para o tutorial seria no docker-compose.yml do serviço de proxy - ao invés de subir o container, conectar nele e rodar o emailproxy, já utilizar o Dockerfile para incluir o run do emaildocker, assim cortando um passo de execução.