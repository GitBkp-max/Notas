---
tags:
  - TI/Tecnologia/DBA
  - TI/Programação/php
---
A exceção **`mysqli_sql_exception`** é lançada quando ocorre um erro na execução de uma consulta SQL utilizando o MySQLi (MySQL Improved) em PHP. Esta exceção é lançada quando há um erro na sintaxe SQL, problemas de conexão com o banco de dados, violações de restrições de chave, entre outros.

Para lidar com essa exceção e exibir informações mais detalhadas sobre o erro, você pode usar um bloco **`try...catch`** em seu código PHP. Aqui está um exemplo de como você pode fazer isso:

```php
<?php
// Configurações de conexão com o banco de dados
$servidor = "localhost";
$usuario = "seu_usuario";
$senha = "sua_senha";
$banco = "seu_banco_de_dados";

// Tentar conectar ao banco de dados
try {
    $conn = new mysqli($servidor, $usuario, $senha, $banco);

    // Verificar se ocorreu algum erro durante a conexão
    if ($conn->connect_error) {
        throw new mysqli_sql_exception("Erro ao conectar ao banco de dados: " . $conn->connect_error);
    }

    // Executar uma consulta SQL (exemplo)
    $sql = "SELECT * FROM tabela_inexistente";
    $resultado = $conn->query($sql);

    // Verificar se ocorreu algum erro durante a execução da consulta
    if (!$resultado) {
        throw new mysqli_sql_exception("Erro ao executar a consulta: " . $conn->error);
    }

    // Processar o resultado da consulta (se necessário)

    // Fechar a conexão com o banco de dados
    $conn->close();
} catch (mysqli_sql_exception $e) {
    // Capturar a exceção e exibir uma mensagem de erro
    echo "Erro: " . $e->getMessage();
}
?>

```

Neste exemplo:

- Estamos tentando conectar ao banco de dados usando **`$conn = new mysqli($servidor, $usuario, $senha, $banco);`**.
- Se houver um erro durante a conexão, uma exceção **`mysqli_sql_exception`** será lançada.
- Da mesma forma, se houver um erro ao executar a consulta SQL, uma exceção **`mysqli_sql_exception`** será lançada.
- No bloco **`catch`**, capturamos a exceção e exibimos uma mensagem de erro usando **`$e->getMessage()`**. Isso fornecerá informações detalhadas sobre o erro que ocorreu.

Certifique-se de ajustar as configurações de conexão com o banco de dados de acordo com o seu ambiente.