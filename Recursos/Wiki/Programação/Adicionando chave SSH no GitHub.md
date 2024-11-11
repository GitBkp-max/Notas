---
tags:
  - TI/Tecnologia/Git-GitHub
---
# Verificar se há chaves SSH

Antes de gerar uma chave SSH, você pode verificar se há outras chaves SSH.
## Sobre chaves SSH

Você pode usar o SSH para executar operações do Git em repositórios. Para obter mais informações, confira "[Sobre o SSH](https://docs.github.com/pt/authentication/connecting-to-github-with-ssh/about-ssh)".
Se você tiver uma chave SSH existente, poderá usar a chave para autenticar as operações do Git por SSH.

## Verificar se há chaves SSH

Antes de gerar uma nova chave SSH, verifique se há chaves existentes no computador local.

> [!NOTE]
> **Observação:** o GitHub aprimorou a segurança removendo tipos de chaves mais antigos e não seguros em 15 de março de 2022.
> 
> Desde essa data, não há mais suporte para as chaves DSA (`ssh-dss`). Não é possível adicionar novas chaves DSA à sua conta pessoal em GitHub.
> 
> As chaves RSA (`ssh-rsa`) com um `valid_after` antes de 2 de novembro de 2021 podem continuar usando qualquer algoritmo de assinatura. As chaves RSA geradas após essa data precisam usar um algoritmo de assinatura SHA-2. Talvez alguns clientes mais antigos precisem ser atualizados para usar as assinaturas SHA-2.

1. Abra Terminal.
    
2. Insira `ls -al ~/.ssh` para ver se as chaves SSH existentes estão presentes.
    
    ```shell
    $ ls -al ~/.ssh
    # Lists the files in your .ssh directory, if they exist
    ```
    
3. Verifique a listagem do diretório para verificar se você já tem uma chave SSH pública. Por padrão, os nomes de arquivos de chaves públicas com suporte para o GitHub são um dos seguintes.
    
    - _id_rsa.pub_
    - _id_ecdsa.pub_
    - _id_ed25519.pub_
    

> [!tip]
>    **Dica**: se você receber um erro indicando que _~/.ssh_ não existe, você não terá um par de chaves SSH existente no local padrão. Você pode criar um novo par de chaves SSH na próxima etapa.

4. Gere uma nova chave SSH ou faça o upload de uma chave existente.
    
    - Se você não tem um par de chave pública e privada compatível ou não deseja usar nenhum que esteja disponível, gere uma nova chave SSH.
        
    - Se um par de chaves pública e privada existente estiver listado (por exemplo, _id_rsa.pub_ e _id_rsa_) que você deseja usar para se conectar ao GitHub, você poderá adicionar a chave ao ssh-agent.
        
        Para saber mais sobre a geração de uma nova chave SSH ou a adição de uma chave existente ao ssh-agent, confira "[Gerando uma nova chave SSH e adicionando-a ao agente SSH](https://docs.github.com/pt/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)".

# Gerando uma nova chave SSH e adicionando-a ao agente SSH

> [!quote]
> Depois de verificar a existência de chaves SSH, é possível gerar uma nova chave SSH para autenticação e adicioná-la ao ssh-agent.

## Sobre frases secretas da chave SSH

Você pode acessar e gravar dados nos repositórios em GitHub usando o protocolo SSH (Secure Shell Protocol). Ao se conectar por meio do SSH, você se autentica usando um arquivo de chave privada no computador local. Para obter mais informações, confira "[Sobre o SSH](https://docs.github.com/pt/authentication/connecting-to-github-with-ssh/about-ssh)".

Ao gerar uma chave SSH, você pode adicionar uma frase secreta para proteger ainda mais a chave. Sempre que você usar a chave, deverá inserir a frase secreta. Se sua chave tiver uma frase secreta e você não quiser inserir a frase secreta sempre que usar a chave, poderá adicionar sua chave ao agente SSH. O agente SSH gerencia suas chaves SSH e lembra sua frase secreta.

Se você ainda não tem uma chave SSH, você deve gerar uma nova chave SSH para usar para a autenticação. Se você não tem certeza se já tem uma chave SSH, você pode verificar se há chaves existentes. Para obter mais informações, confira "[Verificar se há chaves SSH](https://docs.github.com/pt/authentication/connecting-to-github-with-ssh/checking-for-existing-ssh-keys)".

Se você deseja usar uma chave de segurança de hardware para efetuar a autenticação em GitHub, você deverá gerar uma nova chave SSH para a sua chave de segurança de hardware. Você deve conectar a sua chave de segurança de hardware ao seu computador ao efetuar a a sua autenticação com o par de chaves. Para obter mais informações, confira as [notas sobre a versão do OpenSSH 8.2](https://www.openssh.com/txt/release-8.2).

## Gerar uma nova chave SSH

Você pode gerar uma nova chave SSH no computador local. Depois de gerar a chave, você pode adicionar a chave pública à sua conta em GitHub.com para habilitar a autenticação para operações do Git no SSH.

> [!NOTE]
> **Observação:** o GitHub aprimorou a segurança removendo tipos de chaves mais antigos e não seguros em 15 de março de 2022.
> 
> Desde essa data, não há mais suporte para as chaves DSA (`ssh-dss`). Não é possível adicionar novas chaves DSA à sua conta pessoal em GitHub.
> 
> As chaves RSA (`ssh-rsa`) com um `valid_after` antes de 2 de novembro de 2021 podem continuar usando qualquer algoritmo de assinatura. As chaves RSA geradas após essa data precisam usar um algoritmo de assinatura SHA-2. Talvez alguns clientes mais antigos precisem ser atualizados para usar as assinaturas SHA-2.

1. Abra Terminal.

2. Cole o texto abaixo, substituindo o email usado no exemplo pelo seu endereço de email GitHub.

```shell
ssh-keygen -t ed25519 -C "your_email@example.com"
```

> [!NOTE]
> **Observação:** se estiver usando um sistema herdado que não dá suporte ao algoritmo Ed25519, use:
>
> ```shell
>ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
>```

Isto cria uma nova chave SSH, usando o nome de e-mail fornecido como uma etiqueta.

```shell
 > Generating public/private ALGORITHM key pair.
```

Quando for solicitado a inserir um arquivo para salvar a chave, pressione **Enter** para aceitar o local padrão do arquivo. Observe que, se você criou chaves SSH anteriormente, ssh-keygen pode pedir que você reescreva outra chave. Nesse caso, recomendamos criar uma chave SSH personalizada. Para fazer isso, digite o local do arquivo padrão e substitua id_ALGORITHM pelo nome da chave personalizada.

```shell
> Enter a file in which to save the key (/home/YOU/.ssh/id_ALGORITHM):[Press enter]
```

3. No prompt, digite uma frase secreta segura. Para obter mais informações, confira "[Trabalhar com frase secreta da chave SSH](https://docs.github.com/pt/authentication/connecting-to-github-with-ssh/working-with-ssh-key-passphrases)".

```shell
> Enter passphrase (empty for no passphrase): [Type a passphrase]
> Enter same passphrase again: [Type passphrase again]
```
## Adicionar sua chave SSH ao ssh-agent

Antes de adicionar uma nova chave SSH ao agente para gerenciar suas chaves, você deve verificar as chaves SSH existentes e gerado uma nova chave SSH.

1. Inicie o ssh-agent em segundo plano.
```shell
$ eval "$(ssh-agent -s)"
> Agent pid 59566
```

Dependendo do seu ambiente, talvez seja necessário usar um comando diferente. Por exemplo, talvez seja necessário usar o acesso raiz executando `sudo -s -H` antes de iniciar o ssh-agent ou usar `exec ssh-agent bash` ou `exec ssh-agent zsh` para executar o ssh-agent.

2. Adicione sua chave SSH privada ao ssh-agent.

Se você criou sua chave com um nome diferente ou está adicionando uma chave existente que tenha outro nome, substitua _id_ed25519_ no comando pelo nome do arquivo de chave privada.

```shell
ssh-add ~/.ssh/id_ed25519
```

3. Adicione a chave pública SSH à sua conta em GitHub. Para obter mais informações, confira "[Adicionar uma nova chave SSH à sua conta do GitHub](https://docs.github.com/pt/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)".

# Adicionar uma nova chave SSH à sua conta do GitHub

> [!quote]
> Para configurar sua conta em GitHub.com para usar sua chave SSH nova (ou existente), você também precisará adicionar a chave à conta.

## Sobre a adição de chaves SSH à sua conta

Você pode acessar e gravar dados nos repositórios em GitHub usando o protocolo SSH (Secure Shell Protocol). Ao se conectar por meio do SSH, você se autentica usando um arquivo de chave privada no computador local. Para saber mais, confira "[Sobre o SSH](https://docs.github.com/pt/authentication/connecting-to-github-with-ssh/about-ssh)".

Você também pode usar SSH para assinar commits e tags. Para saber mais sobre a assinatura de commit, confira "[Sobre a verificação de assinatura de commit](https://docs.github.com/pt/authentication/managing-commit-signature-verification/about-commit-signature-verification)".

Depois de gerar um par de chaves de SSH, você pode adicionar a chave pública em GitHub.com para permitir que o SSH acesse sua conta.

## Pré-requisitos

Antes de adicionar uma nova chave SSH à sua conta em GitHub.com, conclua as etapas a seguir.

1. Verifique se há chaves SSH existentes. Para obter mais informações, confira "[Verificar se há chaves SSH](https://docs.github.com/pt/authentication/connecting-to-github-with-ssh/checking-for-existing-ssh-keys)".
2. Gere uma nova chave SSH e adicione-a ao agente SSH do computador. Para obter mais informações, confira "[Gerando uma nova chave SSH e adicionando-a ao agente SSH](https://docs.github.com/pt/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)".

## Como adicionar uma nova chave SSH à sua conta

Você pode adicionar uma chave SSH e usá-la para autenticação, assinatura de confirmação ou ambos. Se desejar usar a mesma chave SSH para autenticação e assinatura, será necessário fazer upload da chave duas vezes.

Depois de adicionar uma nova chave SSH de autenticação à sua conta em GitHub.com, você poderá reconfigurar qualquer repositório local para usar o SSH. Para obter mais informações, confira "[Gerenciar repositórios remote](https://docs.github.com/pt/get-started/getting-started-with-git/managing-remote-repositories#switching-remote-urls-from-https-to-ssh)".

1. Copie a chave pública SSH para a sua área de transferência.

Se o seu arquivo de chave pública SSH tiver um nome diferente do código de exemplo, modifique o nome do arquivo para corresponder à sua configuração atual. Ao copiar sua chave, não adicione novas linhas nem espaços em branco.

```shell
$ cat ~/.ssh/id_ed25519.pub
# Then select and copy the contents of the id_ed25519.pub file
# displayed in the terminal to your clipboard
```

> [!tip] Dica:
>  como alternativa, você pode localizar a pasta oculta `.ssh`, abrir o arquivo no seu editor de texto favorito e copiá-lo para a área de transferência.

1. No canto superior direito de qualquer página do GitHub, clique sua foto de perfil e, em seguida, clique em **Configurações**.
2. Na seção "Acesso" da barra lateral, clique em **Chaves SSH e GPG**.
3. Clique em **Nova chave SSH** ou **Adicionar chave SSH**.
4. No campo "Title" (Título), adicione uma etiqueta descritiva para a nova chave. Por exemplo, se estiver usando um laptop pessoal, você poderá chamar essa chave de "Laptop pessoal".
5. Selecione o tipo de chave: autenticação ou assinatura. Para saber mais sobre a assinatura de commit, confira "[Sobre a verificação de assinatura de commit](https://docs.github.com/pt/authentication/managing-commit-signature-verification/about-commit-signature-verification)".
6. No campo "Chave", cole sua chave pública.
7. Clique em **Adicionar chave SSH**.
8. Se solicitado, confirme acesso à sua conta em GitHub. Para obter mais informações, confira "[Modo sudo](https://docs.github.com/pt/authentication/keeping-your-account-and-data-secure/sudo-mode)".