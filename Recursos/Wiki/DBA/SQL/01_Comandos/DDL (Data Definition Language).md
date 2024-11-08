---
aliases: 
tags:
  - Tecnologia/DBA
---
# DDL (Data Definition Language):

## **CREATE**

Usado para criar objetos de banco de dados, como tabelas, índices, visões, procedimentos armazenados, funções, entre outros.

**Sintaxe**

```
CREATE TABLE nome_da_tabela (
    nome_coluna1 tipo_de_dado [restrições],
    nome_coluna2 tipo_de_dado [restrições],
    ...
);
```

**Exemplos de uso**

```
CREATE TABLE usuarios (
    id INT PRIMARY KEY,
    nome VARCHAR(50),
    email VARCHAR(100) UNIQUE,
    idade INT
);
```

---

## **ALTER**

Permite modificar a estrutura de um objeto de banco de dados existente, como uma tabela. Pode ser usado para adicionar, modificar ou remover colunas, alterar o tipo de dados de uma coluna, renomear uma tabela, entre outras alterações.

Sintaxe Básica

```
ALTER TABLE nome_da_tabela
[ADD | MODIFY | DROP] nome_coluna tipo_de_dado [restrições];
```

**Exemplo de uso** (adicionando uma nova coluna)

```
ALTER TABLE usuarios
ADD sobrenome VARCHAR(50);
```

### Argumentos ADD e Modify

**ADD**: O argumento ADD é usado para adicionar uma nova coluna a uma tabela existente.

**Sintaxe:**

```
ALTER TABLE nome_da_tabela
ADD nome_coluna tipo_de_dado [restrições];
```

**Exemplos de uso:**  
Digamos que temos uma tabela chamada de `produtos` e queremos adicionar uma nova coluna chamada `preco` para armazenar os preços dos produtos:

```
ALTER TABLE produtos
ADD preco DECIMAL(10,2);
```

**MODIFY**: O argumento MODIFY é usado para modificar a definição de uma coluna existente em uma tabela, alterando seu tipo de dado ou suas restrições

**Sintaxe:**

```
ALTER TABLE nome_da_tabela
MODIFY nome_coluna novo_tipo_de_dado [novas_restrições];
```

**Exemplos de uso:**  
Suponha que precisamos alternar o tipo de dado da coluna `idade` na tabela `usuario` de INT para SMALLINT:

```
ALTER TABLE usuarios
MODIFY idade SMALLINT;
```

### Outros argumentos de ALTER TABLE:

**DROP**: Usado para remover uma coluna existente de uma tabela

**Sintaxe**:

```
ALTER TABLE nome_da_tabela
DROP COLUMN nome_coluna;
```

**Exemplos de uso:**

```
ALTER TABLE usuarios
DROP COLUMN sobrenome;
```

**RENAME**: Usado para renomear uma tabela ou uma coluna

**Sintaxe para renomear uma tabela:**

```
ALTER TABLE nome_da_tabela
RENAME TO novo_nome_da_tabela;
```

**Exemplos de uso para renomear uma tabela**

```
ALTER TABLE produtos
RENAME TO itens;
```

**Sintaxe para renomear uma coluna:**

```
ALTER TABLE nome_da_tabela
RENAME COLUMN nome_coluna TO novo_nome_coluna;
```

**Exemplo de uso para renomear uma coluna**

```
ALTER TABLE nome_da_tabela
RENAME COLUMN nome_coluna TO novo_nome_coluna;
```

**ADD CONSTRAINT:** Usado para adicionar uma restrição (como chave primaria, chave estrangeira, restrição de verificação, etc.) a uma tabela existente

**Sintaxe**

```
ALTER TABLE nome_da_tabela
ADD CONSTRAINT nome_da_restrição tipo_de_restrição (coluna(s));
```

**Exemplos de uso (adicionando uma chave estrangeira):**

```
ALTER TABLE pedidos
ADD CONSTRAINT fk_cliente_id
FOREIGN KEY (cliente_id) REFERENCES clientes(id);
```

Esses são alguns dos argumentos adicionais que podem ser usados com o comando ALTER TABLE para realizar diversas operações de modificação na estrutura de uma tabela existente. Certifique-se de consultar a documentação do seu SGBD específico para obter uma lista completa de opções suportadas.

---

## **DROP**

Utilizado para remover objetos de banco de dados, como tabelas, índices, visões, procedimentos armazenados, funções, entre outros.

**Sintaxe:**

```
DROP TABLE nome_da_tabela;
```

**Exemplos de uso:**

```
DROP TABLE usuarios;
```

---

## **TRUNCATE**

O comando `TRUNCATE` é usado para remover todos os registros de uma tabela, mas mantendo a estrutura da tabela intacta. Em essência, ele limpa os dados da tabela, mas não elimina a própria estrutura da tabela, como faria o comando `DROP TABLE`.

Ele é útil em situações em que você precisa limpar uma tabela sem excluir e recriá-la. Isso pode ser útil, por exemplo, quando você deseja redefinir o estado de uma tabela para um ponto inicial, após testes ou quando precisa remover todos os registros de uma tabela grande de forma rápida.

No entanto, é importante ter cuidado ao usar o comando `TRUNCATE`, pois ele remove todos os dados da tabela sem possibilidade de recuperação. Certifique-se de que você realmente deseja limpar todos os dados antes de executar esse comando, pois não há como desfazer a operação.

**Sintaxe basica**

```
TRUNCATE TABLE nome_da_tabela;
```

**Exemplos de uso:**

```
TRUNCATE TABLE usuarios;
```