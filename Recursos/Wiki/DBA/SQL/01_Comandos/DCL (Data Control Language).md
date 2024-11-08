---
aliases: 
tags:
  - Tecnologia/DBA
---
# DCL (Data Control Language):

## **GRANT**

O comando GRANT é usado para conceder permissão aos usuários acessar objetos de banco de dados, como tabelas, visões, procedimentos armazenados, etc.

**sintaxe:**

```
GRANT permissões
ON objeto
TO usuário;
```

- **Permissões**: são as permissões que voce deseja conceder, como SELECT, INSERT, UPDATE, DELETE, etc.
- **Objeto**: É o objeto do banco de dados ao qual voce esta concedendo permissão, como uma tabela, visão, procedimento armazenado, etc.
- **Usuario**: É o usuario ou conjunto de usuários aos quais você está concedendo as permissões.

**Exemplos de uso**  
Concedendo permissão de SELECT na tabela clientes para o usuário `usuario1`:

```
GRANT SELECT
ON clientes
TO usuario1;
```

---

## **REVOKE**

O comando REVOKE é usado para revogar as permissões concedidas anteriormente a um usuario

**Sintaxe**

```
REVOKE permissões
ON objeto
FROM usuário;
```

- **Permissão**: São as permissões que voce deseja revogar
- **Objeto**: É o objeto do banco de dados do qual você está revogando as permissões
- **Usuário**: É o usuário ou conjunto de usuários dos quais você está revogando as permissões

**Exemplos de uso**  
Revogando a permissão de SELECT na tabela clientes do usuário usuario1

```
REVOKE SELECT
ON clientes
FROM usuario1;
```