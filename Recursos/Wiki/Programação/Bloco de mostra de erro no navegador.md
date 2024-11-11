---
tags:
  - TI/Programação/php
---
Para informar detalhadamente os erros no console do navegador ao executar scripts PHP, você pode configurar o PHP para exibir mensagens de erro e habilitar a exibição de erros no navegador. Aqui está como fazer isso:

1. **Habilitar exibição de erros no PHP**: Você pode configurar o PHP para exibir erros definindo a diretiva **`display_errors`** como **`On`** no arquivo **`php.ini`**. Se estiver trabalhando em um ambiente de desenvolvimento, isso pode ser útil, mas em um ambiente de produção, é recomendável desativar a exibição de erros para os usuários finais por motivos de segurança.
2. **Habilitar relatórios de erros no PHP**: Você também pode configurar o PHP para relatar todos os erros, não apenas exibi-los. Para fazer isso, defina a diretiva **`error_reporting`** para o nível de relatório de erro desejado, como **`E_ALL`**.
3. **Exibir erros no console do navegador**: Para exibir os erros do PHP no console do navegador, você pode usar a função **`error_log()`** para enviar os erros para o log do servidor da web, que geralmente é exibido no console do navegador. Certifique-se de configurar corretamente o ambiente de desenvolvimento para exibir os logs de erro no console do navegador.

Aqui está um exemplo de como você pode configurar o PHP para exibir e relatar erros no navegador e enviar os erros para o log do servidor da web:

```php
//bloco de analise de erro
ini_set('display_errors', 1);
ini_set('display_startup_errors', 1);
error_reporting(E_ALL);

ini_set('log_errors', 1);
ini_set('error_log', '/var/www/html/controle-de-equipamento/log_erro.log');

function errorHandler($errno, $errstr, $errfile, $errline){
    $error = "PHP Error: [$errno] $errstr in $errfile on line $errline";
    echo "<script>console.error('$error');</script>";
}
set_error_handler("errorHandler");
//bloco e analise de erro
```

Neste exemplo:

- **`ini_set()`** é usado para definir configurações do PHP dinamicamente.
- **`error_reporting(E_ALL)`** define o nível de relatório de erro para exibir todos os tipos de erros.
- **`error_log()`** é usado para enviar os erros para o log do servidor da web.
- **`errorHandler()`** é uma função de manipulação de erros personalizada que exibe os erros no console do navegador usando JavaScript.
- **`set_error_handler()`** é usado para definir a função **`errorHandler()`** como manipulador de erros padrão do PHP.

Certifique-se de substituir **`'/caminho/para/o/arquivo_de_log_php.log'`** pelo caminho real para o arquivo de log do PHP em seu servidor.