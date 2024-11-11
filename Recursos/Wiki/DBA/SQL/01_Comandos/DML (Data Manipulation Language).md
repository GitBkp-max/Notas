---
aliases: 
tags:
  - TI/Tecnologia/DBA/SQL
---
# DML (Data Manipulation Language):

## **SELECT**

O comando SELECT é usado para recuperar dados de uma ou mais tabelas de um banco de dados.

**Sintaxe básica**

```
SELECT coluna1, coluna2, ...
FROM nome_da_tabela
WHERE condição;
```

**Exemplos:**  
Suponha que temos uma tabela chamada `usuarios` com colunas `id`, `nome` e `email`, e queremos recuperar todos os registros:

```
SELECT * 
FROM usuarios;
```

---

## **INSERT**

O comando INSERT é usado para adicionar novas linhas a uma tabela

**Sintaxe Básica**

```
INSERT INTO nome_da_tabela (coluna1, coluna2, ...)
VALUES (valor1, valor2, ...);
```

**Exemplos de uso**  
Adicionando un novo usuário à tabela `usuarios`:

```
INSERT INTO usuarios (nome, email)
VALUES ('João', 'joao@example.com');
```

---

## **UPDATE**

O comando UPDATE é usado para modificar os dados em uma ou mais linhas de uma tabela

**Sintaxe Basica:**

```
UPDATE nome_da_tabela
SET coluna1 = valor1, coluna2 = valor2, ...
WHERE condição;
```

**Exemplos de uso**  
Atualizando o e-mail de um usuário na tabela `usuarios`:

```
UPDATE usuarios
SET email = 'novoemail@example.com'
WHERE id = 1;
```

---

## **DELETE**

O comando DELETE é usado para remover linhas de uma tabela

**Sintaxe basica:**

```
DELETE FROM nome_da_tabela
WHERE condição;
```

**Exemplos de uso**   
Removendo um usuário da tabela `usuarios` com base no ID:

```
DELETE FROM usuarios
WHERE id = 1;
```