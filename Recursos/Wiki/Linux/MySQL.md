---
tags:
  - TI/Tecnologia/DBA/MySQL
  - TI/SO/Linux
---
Claro! Vou te explicar como instalar o MySQL em uma distribuição Linux, em português.

### 1. **Atualizar o sistema**
Antes de começar, é sempre bom garantir que o sistema esteja atualizado. No caso de distribuições baseadas em **Debian/Ubuntu**, use o seguinte comando:

```bash
sudo apt update && sudo apt upgrade -y
```

### 2. **Instalar o MySQL**
Agora, você pode instalar o MySQL com o seguinte comando:

```bash
sudo apt install mysql-server -y
```

Este comando instala o servidor MySQL, juntamente com as ferramentas necessárias para usá-lo.

### 3. **Configurar o MySQL**
Após a instalação, é recomendado rodar o script de segurança do MySQL, que ajuda a configurar o banco de dados de maneira mais segura. Para isso, execute o comando:

```bash
sudo mysql_secure_installation
```

Esse script irá te pedir para definir uma senha para o usuário `root`, remover usuários anônimos, desabilitar o login remoto para o usuário `root` e outras configurações de segurança. O ideal é responder "Y" (sim) para a maioria das perguntas, a não ser que você tenha necessidades específicas.

### 4. **Verificar se o MySQL está rodando**
Para garantir que o MySQL foi instalado e está funcionando corretamente, você pode verificar o status do serviço com o seguinte comando:

```bash
sudo systemctl status mysql
```

Se tudo estiver certo, o status será `active (running)`.

### 5. **Acessar o MySQL**
Agora, você pode acessar o MySQL com o seguinte comando:

```bash
sudo mysql -u root -p
```

Ele vai pedir a senha que você configurou no passo anterior. Após digitar a senha, você estará dentro do prompt do MySQL e poderá começar a criar bancos de dados e executar comandos SQL.

### 6. **(Opcional) Habilitar o MySQL para iniciar automaticamente**
Se, por algum motivo, o MySQL não estiver configurado para iniciar automaticamente quando o sistema reiniciar, você pode habilitar o serviço com o seguinte comando:

```bash
sudo systemctl enable mysql
```

### Conclusão
Agora, o MySQL deve estar instalado e funcionando no seu sistema. Você já pode começar a trabalhar com bancos de dados. Se você tiver mais dúvidas ou precisar de mais detalhes, fique à vontade para perguntar!