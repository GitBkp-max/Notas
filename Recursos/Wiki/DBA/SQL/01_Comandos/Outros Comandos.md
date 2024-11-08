---
tags:
  - Tecnologia/DBA
aliases:
---
# Outros Comandos:

## **DESCRIBE ou DESC**

O comando DESCRIBE ou DESC é usado para mostrar a estrutura de uma tabela, exibindo informações sobre as colunas, seus tipos de dados e outras propriedades.

**Sintaxe**:

```
DESCRIBE nome_da_tabela;
```

ou

```
DESC nome_da_tabela;
```

**Exemplo de uso:**  
Se queremos ver a estrutura da tabela clientes:

```
DESCRIBE clientes;
```

Isso retornará informações sobre as colunas presentes na tabela clientes, como nome da coluna, tipo de dados, tamanho, se é nula ou não, entre outros.

---

## **SHOW**

O comando SHOW é usado para exibir informações sobre bancos de dados, tabelas, índices, usuários, privilégios, entre outros objetos do banco de dados.

**Sintaxe**:  
A sintaxe exata varia dependendo do que você deseja mostrar. Aqui estão alguns exemplos:

- Mostrar todas as tabelas em um banco de dados:

```
SHOW TABBLES;
```

- Mostrar informações sobre uma tabela específica, como seu esquema:

```
SHOW CREATE TABLE nome_da_tabela;
```

- Mostrar informações sobre usuários e privilégios:

```
SHOW GRANTS FOR 'nome_do_usuario'@'localhost';
```

**Exemplos de Uso**  
Para listar todas as tabelas em um banco de dados:

```
SHOW TABLES;
```

Isso retornará uma lista de todas as tabelas presentes no banco de dados atual.

Esses comandos são úteis para obter informações sobre a estrutura das tabelas e outros objetos do banco de dados, bem como para exibir detalhes sobre bancos de dados, usuários e privilégios. Eles são valiosos para entender e explorar o banco de dados em uso.