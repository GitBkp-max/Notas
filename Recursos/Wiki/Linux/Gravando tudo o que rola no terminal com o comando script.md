---
tags:
  - TI/SO/Linux
---
Esta dica é útil para que está instalando algum programa a partir do fonte (_sources_) e todos as mensagens de erros rolam na tela, sem proporcionar a possibilidade de verificar tais erros. Pode ser útil para diversos outros fins que necessitam de manter um controle maior sobre a exibição da tela.

O Linux oferece um podereso comando chamado **`script`**, cuja finalidade é armazenar em log tudo que rola na tela.

Ele possui apenas um único parâmetro que é o nome do arquivo em que ele guardará todas as mensagens. Uma vez que o comando é executado, pode-se continuar a realizar as atividades, como compilar ou rodar um programa e tudo que for exibido na tela será gravado no arquivo. Para finalizar a gravação do log, digita-se **`exit`** e o comando **`script`** é concluído.

```bash
$ script meu_log
Script iniciado, o arquivo é meu_log
$ ls -lha
$ ps aux
$ free -m
$ cat 20080217.txt
$ exit
Script concluído, o arquivo é meu_log
```

No exemplo dado, a princípio, inicia-se o comando **`script`** juntamente com o nome do arquivo a gravar o log (**`meu_log`**). Logo após, tudo que for digitado e exibido na tela do terminal é armazendo no arquivo. Ao digitar **`exit`**, o **`script`** é finalizado, sendo então possível ver o conteúdo do arquivo, com os comandos e suas respectivas saídas.

À primeira vista o comando **`script`** pode ser simples e sem muita aplicabilidade, mas permite gravar a saída de qualquer comando Linux e, com certeza, haverá uma situação que lhe será interessante.