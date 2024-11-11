---
tags:
  - TI/Programação/php
  - TI/Tecnologia/DBA
  - TI/SO/Linux
---
### Passo 1: Verificar se o PHP está instalado
Para o **phpMyAdmin** funcionar com o **Apache**, é necessário que o **PHP** e alguns módulos do **PHP** estejam instalados. Para verificar se o **PHP** está instalado, execute o comando:

```bash
php -v
```

Se o **PHP** não estiver instalado ou a versão não for compatível, você pode instalá-lo com o seguinte comando:

```bash
sudo apt install php php-mbstring php-xml php-mysql libapache2-mod-php -y
```

Este comando instala:
- **php**: A versão principal do PHP.
- **php-mbstring**: Módulo necessário para o **phpMyAdmin**.
- **php-xml**: Módulo necessário para o **phpMyAdmin**.
- **php-mysql**: Módulo que permite ao PHP se comunicar com o MySQL.
- **libapache2-mod-php**: Módulo necessário para rodar o PHP com o Apache.

### Passo 2: Reiniciar o Apache
Após a instalação do PHP e dos módulos necessários, reinicie o servidor Apache para garantir que as mudanças sejam aplicadas:

```bash
sudo systemctl restart apache2
```

### Passo 3: Verificar se o PHP está funcionando
Você pode verificar se o PHP está sendo interpretado corretamente pelo Apache criando um arquivo PHP de teste. Crie o arquivo `info.php` dentro do diretório raiz do Apache:

```bash
sudo nano /var/www/html/info.php
```

Adicione o seguinte conteúdo:

```php
<?php
phpinfo();
?>
```

Salve o arquivo (Ctrl + O) e saia do editor (Ctrl + X).

Agora, abra seu navegador e acesse:

```
http://localhost/info.php
```

Se tudo estiver correto, você verá uma página com informações detalhadas sobre a configuração do PHP.

### Passo 4: Testar o phpMyAdmin novamente
Agora, tente acessar o **phpMyAdmin** novamente no navegador:

```
http://localhost/phpmyadmin
```

Isso deve resolver o problema da tela em branco.

### Passo 5: Remover o arquivo de teste
Depois de testar o PHP, é recomendável excluir o arquivo `info.php` por motivos de segurança:

```bash
sudo rm /var/www/html/info.php
```

### Resumo

1. **Verifique se o PHP está instalado** com `php -v`.
2. **Instale o PHP e os módulos necessários**:

   ```bash
   sudo apt install php php-mbstring php-xml php-mysql libapache2-mod-php -y
   ```

3. **Reinicie o Apache**:

   ```bash
   sudo systemctl restart apache2
   ```

4. **Verifique se o PHP está funcionando** criando um arquivo de teste `info.php`.

5. **Acesse o phpMyAdmin** novamente:

   ```
   http://localhost/phpmyadmin
   ```

Se ainda tiver algum problema, me avise e podemos investigar mais a fundo!