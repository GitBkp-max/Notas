---
aliases: 
tags:
  - TI/Tecnologia/DBA/SQL
---
# TCL (Transaction Control Language):

## **COMMIT**

O comando COMMIT é usado para salvar todas as alterações feitas em uma transação no banco de dados

**sintaxe**

```
COMMIT
```

**Exemplos de uso:**  
Após realizar uma serie de operações de atualização em uma transação, você pode confirmar essas alterações usando o comando COMMIT:

```
UPDATE clientes
SET status = 'ativo'
WHERE id = 1;

COMMIT;
```

---

## **ROLLBACK**

O comando é usado para desfazer todas as alterações feitas em uma transação e restaurar o estado do banco de dados para o inicio da transação

**Sintaxe**:

```
ROLLBACK;
```

**Exemplos de uso:**  
Se algo der errado durante uma transação, você pode desfazer todas as alterações feitas até o momento usando o comando ROLLBACK

```
BEGIN TRANSACTION;

UPDATE conta
SET saldo = saldo - 100
WHERE cliente_id = 1;

-- Algum erro ocorreu
ROLLBACK;
```

---

## **SAVEPOINT**

O comando é usado para definir um ponto dentro de uma transação a parttir do qual é possivel reverter as alterações posteriormente

**Sintaxe**:

```
SAVEPOINT nome_do_savepoint;
```

**Exemplos de uso:**  
Durante uma transformação, você pode definir um savepoint antes de executar uma operação critica. Se necessário, você pode voltar para este ponto mais tarde usando o comando **ROLLBACK TO SAVEPOINT**:

```
BEGIN TRANSACTION;

UPDATE produtos
SET quantidade = quantidade - 10
WHERE id = 1;

SAVEPOINT antes_atualizacao;

UPDATE produtos
SET quantidade = quantidade + 10
WHERE id = 2;

-- Se algo der errado, podemos voltar para o savepoint antes da atualização
ROLLBACK TO SAVEPOINT antes_atualizacao;
```

---

## **SET TRANSACTION**

O comando é usado para definir propriedades de transação,c omo isolamento e controle de concôrrencia.

**Sintaxe**:

```
SET TRANSACTION [propriedade1, propriedade2, ...];
```

**Exemplos de uso:**  
Definindo o nível de isolamento da transação como **SERIALIZABLE**

```
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
```

Isso garante que a transação tenha o nivel mais alto de isolamento, impedindo que outras transações acessem os mesmo dados até que a transação atual seja concluída