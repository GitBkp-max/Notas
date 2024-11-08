---
tags:
  - git
---
O comando `git rebase` tem a reputação de ser o feitiço mágico do Git do qual os iniciantes devem ficar longe, entretanto, ele pode facilitar muito a vida da equipe de desenvolvimento quando usado com cuidado. Neste artigo, vamos comparar o `git rebase` com o comando `git merge` relacionado e identificar todas as oportunidades potenciais para incorporar o rebase ao fluxo de trabalho típico do Git.

## Conceptual overview

A primeira coisa a ser entendida sobre o `git rebase` é que resolve o mesmo problema que o `git merge`. Ambos os comandos são desenvolvidos para integrar alterações de uma ramificação para outra—eles só fazem isto de formas muito diferentes.

Pense no que acontece quando você começa a trabalhar em uma nova função em uma ramificação dedicada e, em seguida, outro membro da equipe atualiza a ramificação `main` com novos commits. O resultado é um histórico bifurcado, e qualquer pessoa que já usou o Git como ferramenta de colaboração sabe disso.

![Um histórico de confirmação bifurcada](https://wac-cdn.atlassian.com/dam/jcr:1523084b-d05a-4f5a-bd1a-01866ec09ca3/01%20A%20forked%20commit%20history.svg?cdnVersion=2412)

Agora, digamos que os novos commits no `main` sejam relevantes para a função em que você está trabalhando. Para incorporar os novos commits na ramificação de `funções`, você tem duas opções: merge ou rebase.

### The merge option

A opção mais fácil é realizar o merge da ramificação `main` com a ramificação de funções usando algo parecido com o seguinte:

```bash
git checkout feature
git merge main
```

Ou você pode condensar isso em uma linha de comando:

```bash
git merge feature main
```

Assim você cria uma nova “confirmação de mesclagem” na ramificação de `funções` que se une aos históricos de ambos os branches, dando a você uma estrutura de ramificação semelhante ao seguinte:

![Merging main into feature branch](https://wac-cdn.atlassian.com/dam/jcr:4639eeb8-e417-434a-a3f8-a972277fc66a/02%20Merging%20main%20into%20the%20feature%20branh.svg?cdnVersion=2412)

A mesclagem é legal porque é uma operação _não destrutiva_. As ramificações existentes não são alteradas de forma alguma. Isso evita todas as possíveis armadilhas do rebase (discutidas abaixo).

Por outro lado, também indica que a ramificação de `funções` vai ter um commit de merge estranho toda vez que você precisar incorporar alterações upstream. Se a `main` for muito ativa, pode poluir um pouco o histórico da ramificação de funções. Embora seja possível mitigar o problema com opções avançadas de `git log`, pode ficar difícil para que outros desenvolvedores entendam o histórico do projeto.

### The rebase option

Como uma alternativa ao merge, você pode fazer o rebase da ramificação `feature` na ramificação `main` usando os comandos a seguir:

```bash
git checkout feature
git rebase main
```

Essa ação move a ramificação `feature` inteira para começar na ponta da ramificação `main`, incorporando com efetividade todos os novos commits no `main`. Porém, em vez de usar um commit de merge, o rebase _reescreve_ o histórico do projeto criando novos commits para cada commit na ramificação original.

![Rebasing feature branch into main](https://wac-cdn.atlassian.com/dam/jcr:3bafddf5-fd55-4320-9310-3d28f4fca3af/03%20Rebasing%20the%20feature%20branch%20into%20main.svg?cdnVersion=2412)

O maior benefício do rebase é que você obtém um histórico de projeto muito mais limpo. Primeiro, ele elimina commits de mesclagem desnecessários exigidos pelo `git merge`. Segundo, como você pode ver no diagrama acima, o rebase também resulta em um histórico de projeto perfeitamente linear—você pode seguir a ponta da `funcionalidade` até o começo do projeto sem quaisquer bifurcações. Isso facilita a navegação pelo projeto com comandos como `git log`, `git bisect` e `gitk`.

Porém, há duas desvantagens para este histórico puro de commit: segurança e rastreabilidade. Se você não seguir a [Regra de ouro do rebase](https://www.atlassian.com/br/git/tutorials/merging-vs-rebasing#the-golden-rule-of-rebasing), reescreve o histórico do projeto pode ser potencialmente catastrófico para o seu fluxo de trabalho colaborativo. E, de modo menos importante, o rebase perde o contexto fornecido por um commit de mesclagem—você não consegue ver quando as alterações upstream foram incorporadas na funcionalidade.

### Interactive rebasing

O rebase interativo dá a você a oportunidade de alterar commits à medida que eles são movidos para a nova ramificação, e é ainda mais poderoso do que um rebase automatizado, já que oferece o controle completo sobre o histórico de commit da ramificação. O normal é usar essa ação para limpar um histórico bagunçado antes de fazer o merge de uma ramificação de funções na `main`.

Para iniciar uma sessão interativa de rebase, passe a opção `i` para o comando `git rebase`:

```bash
git checkout feature
git rebase -i main
```

Isso vai abrir um editor de texto, listando todas as confirmações que estão prestes a serem movidas:

```bash
pick 33d5b7a Message for commit #1
pick 9480b3d Message for commit #2
pick 5c67e61 Message for commit #3
```

Essa listagem traz a definição exata de qual vai ser o aspecto do branch depois da execução do rebase. Ao alterar o comando `pick` e/ou reordenar as entradas, é possível fazer com que o histórico do branch se pareça com o que você quiser. Por exemplo, se o 2º commit corrigir um problema pequeno no 1º commit, você pode condensar os dois em um único commit com o comando `fixup`:

```bash
pick 33d5b7a Message for commit #1
fixup 9480b3d Message for commit #2
pick 5c67e61 Message for commit #3
```

Quando você salva e fecha o arquivo, o Git vai executar o rebase de acordo com suas instruções, resultando em um histórico de projeto parecido com o seguinte:

![Como suprimir uma confirmação com um rebase interativo](https://wac-cdn.atlassian.com/dam/jcr:a712e238-6cb9-4c8c-8ef7-1975dca49be3/04%20Squashing%20a%20commit%20with%20an%20interactive%20rebase.svg?cdnVersion=2412)

Eliminar commits insignificantes como estes faz com que o seu histórico de funcionalidades fique mais fácil de entender. Isso é algo que o `git merge` simplesmente não consegue fazer.

## The golden rule of rebasing

Depois que você entende o que é o rebase, a coisa mais importante a ser aprendida é quando _não_ fazê-lo. A regra de ouro do `git rebase` é nunca usá-lo em ramificações _públicas_.

Por exemplo, pense no que aconteceria se você fizesse rebase da `main` na ramificação `feature`:

![Rebasing the main branch](https://wac-cdn.atlassian.com/dam/jcr:2908e0e6-f74b-4425-b5d2-f5eca8cfcd99/05%20Rebasing%20the%20main%20branch.svg?cdnVersion=2412)

O rebase move todos os commits na ramificação `main` para a ponta de `feature`. O problema é que essa ação só aconteceu no _seu_ repositório. Todos os outros desenvolvedores ainda estão trabalhando com a ramificação `main` original. Como o rebase resulta em commits novos, o Git vai pensar que o histórico da ramificação `main` divergiu do de todas as outras pessoas.

A única forma de sincronizar as duas ramificações `main` é uma nova união por merge, o que resulta em um commit de merge extra _e_ em dois conjuntos de commits que contenham as mesmas alterações (as originais e aquelas da ramificação de rebase). Não é nem preciso dizer que esta é uma situação muito confusa.

Então, antes de executar o `git rebase`, sempre pergunte a si mesmo: “Mais alguém está trabalhando com este branch?” Se a resposta for sim, tire suas mãos do teclado e comece a pensar sobre uma forma não destrutiva de fazer suas alterações (por exemplo, o comando `git revert`). Caso contrário, você pode reescrever o histórico como quiser.

### Force-pushing

Se você tentar colocar a ramificação `main` do rebase em um repositório remoto, o Git não vai deixar você concluir a ação, porque ele entra em conflito com a ramificação `main` remota. Entretanto, você pode forçar a colocação, com a marcação `--force`, desta forma:

```bash
# Be very careful with this command! git push --force
```

Assim, você substitui a ramificação `main` remota para corresponder à de rebase do repositório e torna as coisas muito confusas para o resto da equipe. Então, tome muito cuidado para usar este comando apenas quando souber com exatidão o que está fazendo.

Uma das únicas vezes que você deve forçar a colocação é quando você executou uma limpeza local _depois_ de colocar uma ramificação de funcionalidade privada em um repositório remoto (por exemplo, para fins de backup). Isso é como dizer "Opa, eu não queria de fato colocar aquela versão original da ramificação de funcionalidade. Use a atual no lugar dela". Novamente, é importante que ninguém esteja trabalhando fora dos commits da versão original da ramificação de funcionalidade.

## Workflow walkthrough

O rebase pode ser incorporado ao seu fluxo de trabalho existente do Git na medida em que isso for confortável para sua equipe. Nesta seção, vamos dar uma olhada nos benefícios que o rebase pode oferecer nos vários estágios de desenvolvimento de uma função.

A primeira etapa em qualquer fluxo de trabalho que utilize o `git rebase` é criar um branch dedicado para cada função. Assim você tem a estrutura de ramificação necessária para utilizar o rebase com segurança:

![Como desenvolver uma função em uma ramificação dedicada](https://wac-cdn.atlassian.com/dam/jcr:88bfb19f-81ca-4202-b5d1-f2d8fa8778f2/06%20Developing%20a%20feature%20in%20a%20dedicated%20branch.svg?cdnVersion=2412)

### Local cleanup

Uma das melhores maneiras de incorporar o rebase em seu fluxo de trabalho é limpar as funções locais, em andamento. Ao executar periodicamente um rebase interativo, você pode assegurar que cada confirmação na função seja focada e útil. Isso permite escrever seu código sem se preocupar em quebrá-lo em confirmações isoladas—é possível corrigi-lo depois do fato.

Ao chamar `git rebase`, você tem duas opções para a nova base: a ramificação pai da função (por exemplo, `main`), ou um commit anterior na funções. Você viu um exemplo da primeira opção na seção _Rebase interativo_. A segunda opção é boa quando você só precisa corrigir os últimos commits. Por exemplo, o comando a seguir começa em um rebase interativo apenas dos últimos três commits.

```bash
git checkout feature git rebase -i HEAD~3
```

Ao especificar `HEAD~3` como a nova base, você não está movendo de fato a ramificação—você apenas está reescreve interativamente os 3 commits que a seguem. Observe que isso _não_ incorporará alterações upstream na ramificação de `funcionalidade`.

![Como fazer o rebase em Head~3](https://wac-cdn.atlassian.com/dam/jcr:51d9f126-1fc1-4c6c-ba52-c4085cd59002/07%20Rebasing%20into%20Head-3.svg?cdnVersion=2412)

Se você deseja reescrever a funcionalidade inteira usando este método, o comando `git merge-base` pode ser útil para encontrar a base original da ramificação de `funcionalidade`. O seguinte retorna o ID de commit da base original, que você pode, então, passar para o `git rebase`:

```bash
git merge-base feature main
```

Este uso do rebase interativo é uma ótima maneira de introduzir o `git rebase` no seu fluxo de trabalho, já que ele afeta somente as ramificações locais. A única coisa que os outro desenvolvedores verão é o seu produto final, que deve ser um histórico de ramificação de funcionalidade limpo e fácil de seguir.

Mas, novamente, isso apenas funciona para ramificações de _funcionalidade_ privadas. Se você está colaborando com outros desenvolvedores pela mesma ramificação de funcionalidade, essa ramificação é _pública_, e você não tem permissão para reescrever seu histórico.

Não existe uma alternativa de `git merge` para limpar commits locais com um rebase interativo.

### Incorporating upstream changes into a feature

Na seção _Visão geral conceitual_, vimos como uma ramificação de funções pode incorporar alterações de upstream da ramificação `main` usando `git merge` ou `git rebase`. O merge é uma opção segura que preserva o histórico inteiro do repositório, enquanto o rebase cria um histórico linear movendo a ramificação de funções para a ponta da ramificação `main`.

Este uso de `git rebase` é semelhante a uma limpeza local (e pode ser realizado ao mesmo tempo), mas, no processo, incorpora esses commits de upstream da ramificação `main`.

Saiba que é legal fazer rebase em uma ramificação remota, em vez de na `main`. Essa tarefa pode acontecer quando você colabora na mesma função com outro desenvolvedor e precisa incorporar as mudanças dele no repositório.

Por exemplo, se você e outro desenvolvedor chamado John adicionaram commits à ramificação de `funções`, o aspecto do seu repositório pode ser semelhante ao seguinte depois de procurar a ramificação de `funções` remoto do repositório de John:

![Como colaborar na mesma ramificação de funções](https://wac-cdn.atlassian.com/dam/jcr:0bb661aa-361d-47ba-8c7b-00b3be0546cb/08.svg?cdnVersion=2412)

É possível resolver esta bifurcação da mesma forma que você integra alterações upstream da ramificação `main`: faça merge do `feature` local com `john/feature` ou faça rebase do `feature` local na ponta de `john/feature`.

![Mesclagem vs. rebase em uma ramificação remota](https://wac-cdn.atlassian.com/dam/jcr:1896adb1-5d49-419a-9b50-3a36adac186c/09.svg?cdnVersion=2412)

Observe que esse rebase não viola a _Regra primordial de rebase_, pois apenas os commits de `funções` locais estão sendo movidos — tudo antes deles está intocado. É como se dissesse: “adicione minhas alterações ao que John já fez”. Na maioria das circunstâncias, fazer assim é mais intuitivo do que sincronizar com a ramificação remota por meio de uma confirmação de mesclagem.

Por padrão, o comando `git pull` executa um merge, mas você pode forçar a integração do branch remoto a um rebase passando para ele a opção `--rebase`.

### Reviewing a feature with a pull request

Se você usar solicitações pull como parte do seu processo de revisão de código, precisará evitar usar `git rebase` depois de criar a solicitação pull. Assim que fizer a solicitação pull, outros desenvolvedores olharão para os seus commits, o que significa que eles são uma ramificação _pública_. Reescrever o histórico dele impossibilitaria que o Git e seus companheiros de equipe rastreassem qualquer commit de acompanhamento adicionado à funcionalidade.

Quaisquer alterações de outros desenvolvedores precisam ser incorporadas com `git merge` em vez de com `git rebase`.

Por este motivo, geralmente é uma boa ideia limpar seu código com um rebase interativo _antes_ de enviar sua solicitação pull.

### Integrating an approved feature

Depois que a equipe aprova a função, você tem a opção de fazer rebase dele na ponta da ramificação `principal` antes de usar `git merge` para integrar a função na base do código principal.

Esta é uma situação semelhante a incorporar alterações upstream em uma ramificação de funções, mas como você não tem permissão para reescrever commits na ramificação `main`, talvez seja preciso usar `git merge` para integrar a função. No entanto, ao executar um rebase antes de merge, você vai ter a certeza de que o merge vai ser de avanço rápido, o que resulta em um histórico bem linear. Além disso, você terá a chance de fazer squash em qualquer commit de acompanhamento adicionado durante uma pull request.

![Integrating a feature into main with and without a rebase](https://wac-cdn.atlassian.com/dam/jcr:412813ee-381f-42f6-8b43-79760d2da0dc/08-10%20Integrating%20a%20feature%20into%20master%20with%20and%20without%20a%20rebase.svg?cdnVersion=2412)

Se você não estiver totalmente confortável com o `git rebase`, você pode sempre executar o rebase em uma ramificação temporária. Desta forma, se você bagunçar acidentalmente o histórico da funcionalidade, você poderá verificar a ramificação original e tentar novamente. Por exemplo:

```bash
git checkout feature
git checkout -b temporary-branch
git rebase -i main
# [Clean up the history]
git checkout main
git merge temporary-branch
```

## Resumo

---

E isto é tudo o que você precisa saber para começar a usar rebase nas suas ramificações. Se preferir um histórico limpo, linear e sem commits de mesclagem desnecessários, você deve usar `git rebase` em vez de `git merge` ao integrar alterações de outra ramificação.

Por outro lado, se quiser preservar o histórico completo do seu projeto e evitar o risco de reescrever commits públicos, você pode inserir o `git merge`. Qualquer uma das opções é válida, mas pelo menos agora você tem a opção de aproveitar os benefícios do `git rebase`.