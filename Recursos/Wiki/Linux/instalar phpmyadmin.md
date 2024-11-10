---
tags:
  - Programação/php
  - Tecnologia/DBA
  - SO/Linux
---
Para instalar o **phpMyAdmin** em uma distribuição Linux (usando **Ubuntu** ou **Debian**, que são as mais comuns), você pode seguir os passos abaixo. O **phpMyAdmin** é uma interface web popular para gerenciar o MySQL via navegador, o que facilita bastante o trabalho com bancos de dados.

### 1. **Atualizar o sistema**
Primeiro, é uma boa prática garantir que seu sistema esteja atualizado. Abra o terminal e execute:

```bash
sudo apt update && sudo apt upgrade -y
```

### 2. **Instalar o phpMyAdmin**
Agora, você pode instalar o **phpMyAdmin**. Para isso, execute o seguinte comando:

```bash
sudo apt install phpmyadmin -y
```

Durante a instalação, o sistema pedirá algumas configurações:

- **Escolher o servidor web**: Quando perguntado "Escolha o servidor web para configurar", selecione **Apache2** (usando as setas e pressionando a tecla de espaço para marcar) e depois pressione **Enter**. O phpMyAdmin funciona com o Apache.
  
- **Configurar o banco de dados**: Durante a instalação, você também será perguntado se deseja configurar o banco de dados para o phpMyAdmin. Escolha a opção **Sim** e depois será solicitado a inserir uma senha para o usuário de administrador do MySQL.

### 3. **Habilitar o Apache para phpMyAdmin**
Após a instalação, o phpMyAdmin pode precisar de um pequeno ajuste para funcionar corretamente no servidor web Apache.

Primeiro, você precisa habilitar o arquivo de configuração do phpMyAdmin no Apache com o seguinte comando:

```bash
sudo phpenmod mbstring
sudo systemctl restart apache2
```

Esses comandos ativam o módulo `mbstring` (necessário para alguns recursos do phpMyAdmin) e reiniciam o Apache.

### 4. **Acessar o phpMyAdmin**
Agora, você pode acessar o **phpMyAdmin** através de seu navegador. Basta digitar o seguinte endereço na barra de endereços do navegador:

```
http://localhost/phpmyadmin
```

Se estiver acessando de outra máquina, substitua `localhost` pelo IP do servidor.

### 5. **Login no phpMyAdmin**
Na tela de login do **phpMyAdmin**, você deve entrar com o nome de usuário e senha do MySQL que você configurou anteriormente (geralmente, o usuário é `root` ou outro que você tenha criado).

### 6. **Ajustes de Segurança (opcional)**
É altamente recomendável realizar algumas configurações para aumentar a segurança do seu **phpMyAdmin**, como restringir o acesso ao **phpMyAdmin** por IP ou usar autenticação adicional. Por exemplo, para configurar o acesso apenas a partir de um IP específico, edite o arquivo de configuração do Apache:

```bash
sudo nano /etc/apache2/conf-available/phpmyadmin.conf
```

Dentro do arquivo, você pode adicionar ou ajustar as diretivas de acesso. Após editar, não se esqueça de reiniciar o Apache para aplicar as mudanças:

```bash
sudo systemctl restart apache2
```

### 7. **(Opcional) Usar HTTPS**
Se você estiver usando o **phpMyAdmin** em um ambiente de produção, é altamente recomendado configurar um certificado SSL para garantir que os dados sejam transmitidos de forma segura. Você pode fazer isso com o **Let's Encrypt** ou qualquer outro provedor de SSL.

---

### Resumo dos Comandos

1. Atualizar o sistema:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

2. Instalar o **phpMyAdmin**:
   ```bash
   sudo apt install phpmyadmin -y
   ```

3. Ativar o módulo `mbstring` para o PHP:
   ```bash
   sudo phpenmod mbstring
   sudo systemctl restart apache2
   ```

4. Acessar o **phpMyAdmin**:
   Abra o navegador e acesse:
   ```
   http://localhost/phpmyadmin
   ```

---

Agora você tem o **phpMyAdmin** instalado e configurado no seu servidor Linux! Se precisar de mais alguma coisa ou tiver algum problema, me avise!

---

O erro que você está recebendo, **"Autenticação sem uma senha é proibida pela configuração (veja AllowNoPassword)"**, ocorre porque a autenticação sem senha para o usuário `root` no MySQL foi desabilitada, por questões de segurança. 

Vamos resolver esse problema configurando uma senha para o usuário `root` no MySQL. Siga os passos abaixo para corrigir isso.

### Passo 1: Acessar o MySQL como `root` sem senha
Se o seu MySQL está configurado sem senha para o usuário `root`, você pode acessar o banco de dados diretamente usando o seguinte comando (com privilégios administrativos):

```bash
sudo mysql -u root
```

Este comando deve te permitir acessar o prompt do MySQL sem pedir senha, já que o `root` não tem senha configurada no momento.

### Passo 2: Configurar uma senha para o `root`
Uma vez dentro do prompt do MySQL, execute os seguintes comandos para definir uma senha para o usuário `root`:

1. **Escolher uma senha**: Vamos, por exemplo, configurar a senha como `novaSenha`. Mas sinta-se à vontade para escolher a senha que preferir.
   
2. **Atualizar a senha**: Execute o seguinte comando no MySQL:

   ```sql
   ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'novaSenha';
   ```

   **Nota**: Substitua `novaSenha` pela senha que você deseja configurar.

3. **Aplicar as mudanças**:
   Após definir a senha, você precisa garantir que as alterações sejam salvas. Para isso, execute o seguinte comando:

   ```sql
   FLUSH PRIVILEGES;
   ```

4. **Sair do MySQL**:

   ```sql
   EXIT;
   ```

### Passo 3: Testar o acesso com a nova senha
Agora que você configurou a senha para o usuário `root`, tente acessar o MySQL novamente, mas agora usando a senha que você acabou de configurar:

```bash
sudo mysql -u root -p
```

O sistema vai pedir a senha. Insira a senha que você configurou (por exemplo, `novaSenha`) e você deve conseguir acessar o MySQL normalmente.

### Passo 4: Ajustar o phpMyAdmin
Se você estiver tentando acessar o **phpMyAdmin** e estava usando o **root** sem senha, agora você deve atualizar a configuração do **phpMyAdmin** para usar a nova senha.

1. Edite o arquivo de configuração do **phpMyAdmin**:

   ```bash
   sudo nano /etc/phpmyadmin/config-db.php
   ```

2. Encontre a linha que contém a senha do MySQL para o `root`, e altere para a nova senha:

   ```php
   $cfg['Servers'][$i]['password'] = 'novaSenha';
   ```

3. Salve e feche o arquivo.

4. Reinicie o Apache para garantir que todas as mudanças sejam aplicadas:

   ```bash
   sudo systemctl restart apache2
   ```

### Conclusão
Agora o usuário `root` do MySQL tem uma senha configurada, e você deve conseguir fazer login tanto pelo **terminal** quanto pelo **phpMyAdmin** sem problemas. Se você encontrar algum outro erro ou tiver mais dúvidas, me avise!