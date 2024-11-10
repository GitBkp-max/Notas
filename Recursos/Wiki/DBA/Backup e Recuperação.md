---
tags:
  - Tecnologia/DBA
aliases:
---
02/04/2024 20:06  
[Aula do material usado pt.1](https://www.youtube.com/watch?v=Qul2b_MVq4o "https://www.youtube.com/watch?v=Qul2b_MVq4o")  
[Aula do material usado pt.2](https://www.youtube.com/watch?v=KmYcxZrJ9Jk "https://www.youtube.com/watch?v=KmYcxZrJ9Jk")

# Backup e Restore de banco de dados

- Backup e Restore Manual
- Backup Automático
- Criação e Permissão de Usuários

---

## BACKUP

- Existe duas formas de realizar o **backup** e a **restauração** de banco de dados no [[Artigos/MySQL]]:
    - **Manual pelo [[Sistemas de Gerenciamento de Banco de Dados (SGBD)]]**
    - **Linha de comando no Sistema Operacional**
- Por linha de comando é possível criar um _script_ no Sistema Operacional para executar o backup em períodos determinados de forma automática

### Passo a passo de Backup pelo [[MySQL Workbench]]

1. Abra o MySQL Workbench e conecte-se ao servidor MySQL ao qual você deseja fazer o backup.
2. No painel lateral esquerdo, clique na instância do servidor à qual você deseja fazer o backup.
3. No menu superior, vá para "Server" e escolha "Data Export".
4. Na nova janela que se abre, você verá uma lista de bancos de dados disponíveis no servidor MySQL. Selecione o banco de dados ou os bancos de dados que deseja fazer backup.
5. Escolha o local onde deseja salvar o arquivo de backup. Você pode optar por salvar o arquivo localmente em seu computador ou em um local remoto.
6. Escolha o formato de saída do backup. O MySQL Workbench oferece várias opções de formato, incluindo [[Linguagem SQL]], CSV, JSON, entre outros. Se você estiver fazendo um backup completo do banco de dados, o formato SQL é comum.
7. Clique em "Start Export" para iniciar o processo de backup. O MySQL Workbench começará a exportar os dados do banco de dados selecionado para o arquivo de backup no local especificado.
8. Aguarde até que o processo de exportação seja concluído. Isso pode levar algum tempo, dependendo do tamanho do banco de dados e da velocidade da conexão.
9. Uma vez concluído, você receberá uma mensagem indicando que o backup foi bem-sucedido. Você pode fechar a janela de exportação de dados e verificar o arquivo de backup no local especificado.

É importante lembrar de realizar backups regularmente para garantir a segurança dos seus dados. Além disso, é uma boa prática testar periodicamente a restauração dos backups para garantir que eles estejam funcionando corretamente em caso de necessidade de recuperação.

### Passo a passo de Backup pelo MySQL Workbench

1. **Abra o MySQL Workbench** e conecte-se ao servidor MySQL no qual você deseja importar o arquivo.
    
2. No painel lateral esquerdo, clique na instância do servidor à qual você deseja importar o arquivo.
    
3. No menu superior, vá para "Server" e escolha **"Data Import"**.
    
4. Na nova janela que se abre, você verá uma lista de opções para importar dados. Selecione a opção que corresponde ao formato do arquivo que você deseja importar (por exemplo, SQL, CSV, JSON, etc.).
    
5. Escolha o **arquivo** que deseja importar clicando em "Select File". Navegue até o local onde o arquivo está armazenado em seu computador ou em um local remoto e selecione-o.
    
6. Se necessário, você pode ajustar as opções de importação, como o método de tratamento de erros e a codificação do arquivo.
    
7. Clique em **"Start Import"** para iniciar o processo de importação. O MySQL Workbench começará a importar os dados do arquivo selecionado para o banco de dados especificado.
    
8. Aguarde até que o processo de importação seja concluído. Isso pode levar algum tempo, dependendo do tamanho do arquivo e da complexidade dos dados.
    
9. Uma vez concluído, você receberá uma mensagem indicando que a importação foi bem-sucedida. Você pode fechar a janela de importação de dados e verificar se os dados foram importados corretamente no banco de dados.
    

Lembre-se de verificar se os dados foram importados corretamente e se não houve problemas durante o processo. É recomendável fazer um backup do banco de dados antes de realizar a importação para evitar a perda de dados em caso de erros.

---

## Permissões de usuarios

No MySQL Workbench, as funções administrativas são geralmente associadas aos privilégios e permissões concedidos aos usuários do banco de dados MySQL. O MySQL Workbench oferece uma interface gráfica para administrar esses privilégios, mas as funções administrativas em si são mais uma função do próprio servidor MySQL.

Algumas das principais funções administrativas que podem ser atribuídas aos usuários do MySQL incluem:

1. **Superusuário (Superuser)**: Este é o nível mais alto de privilégio no MySQL. Os superusuários têm controle total sobre o servidor MySQL e podem executar qualquer operação.
    
2. **Administrador de Banco de Dados (Database Administrator)**: Este usuário tem permissões para gerenciar bancos de dados, tabelas e índices, bem como realizar operações de backup e restauração.
    
3. **Administrador de Segurança (Security Administrator)**: Este usuário é responsável por gerenciar os privilégios e as permissões dos usuários do MySQL, incluindo a criação, modificação e exclusão de contas de usuário.
    
4. **Administrador de Sistemas (System Administrator)**: Este usuário tem permissões para configurar e gerenciar o servidor MySQL, incluindo configurações de sistema, redes e armazenamento.
    
5. **Administrador de Replicação (Replication Administrator)**: Este usuário tem permissões para configurar e gerenciar a replicação de dados entre servidores MySQL.
    
6. **Administrador de Backup (Backup Administrator)**: Este usuário é responsável por configurar e gerenciar operações de backup e restauração de dados no servidor MySQL.
    

É importante notar que essas funções administrativas podem ser personalizadas e combinadas de acordo com os requisitos específicos do ambiente MySQL. Além disso, é crucial seguir os princípios de segurança ao atribuir privilégios aos usuários, garantindo que apenas as pessoas autorizadas tenham acesso aos recursos do banco de dados necessários para realizar suas tarefas.

---

## BACKUP AUTOMATIZADO LINUX

Para criar um script de backup automatizado no MySQL no Linux, você pode seguir os passos abaixo:

1. **Crie um script de backup**: Crie um novo arquivo de script em shell (por exemplo, `backup_mysql.sh`) usando um editor de texto como o `nano` ou `vim`. Você pode armazenar este script em um local seguro, como `/usr/local/bin/`.
    
2. **Edite o script**: Adicione os comandos necessários para realizar o backup do banco de dados [[Artigos/MySQL]]. Você pode usar o utilitário `mysqldump` para isso. Aqui está um exemplo de script básico para fazer um backup de todos os bancos de dados MySQL:
    

```
#!/bin/bash

# Defina as variáveis para o backup
USER="seu_usuario"
PASSWORD="sua_senha"
OUTPUT_DIR="/caminho/para/salvar/os/backups"
DATE=$(date +"%Y%m%d%H%M%S")

# Comando mysqldump para criar o backup
mysqldump --user=$USER --password=$PASSWORD --all-databases > $OUTPUT_DIR/backup_$DATE.sql

# Compactar o arquivo de backup
gzip $OUTPUT_DIR/backup_$DATE.sql
```

Certifique-se de substituir `"seu_usuario"`, `"sua_senha"` e `"/caminho/para/salvar/os/backups"` pelos detalhes de autenticação do seu banco de dados MySQL e pelo diretório onde deseja salvar os backups.

3. **Permissões de execução**: Torne o script executável usando o seguinte comando:

```
chmod +x /caminho/do/script/backup_mysql.sh
```

4. **Agende o backup**: Use o `cron` para agendar a execução do script de backup em intervalos regulares. Edite o cron jobs digitando `crontab -e` e adicione uma linha como esta para executar o script de backup todos os dias às 2 da manhã:

```
0 2 * * * /caminho/do/script/backup_mysql.sh
```

5. **Teste o script**: Execute manualmente o script de backup para garantir que ele esteja funcionando conforme o esperado:

```
/caminho/do/script/backup_mysql.sh
```

Isso é tudo! Com esses passos, você criou um script de backup automatizado para o MySQL no Linux. Certifique-se de monitorar regularmente seus backups para garantir que estejam sendo criados corretamente e que seus dados estejam seguros.