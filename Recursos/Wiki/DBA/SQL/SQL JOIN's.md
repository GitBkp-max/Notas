---
tags:
  - TI/Tecnologia/DBA/SQL
---
# SQL JOIN's

A imagem apresenta diferentes tipos de SQL JOINs, utilizados para combinar registros de duas tabelas ( A e B ) com base em uma condição comum. Aqui está uma tradução e explicação detalhada de cada tipo de JOIN mostrado na imagem:

---
## INNER JOIN
 	
```sql
SELECT *  
FROM A  
INNER JOIN B ON A.key = B.key
```
  Explicação: Retorna apenas os registros que têm valores correspondentes nas duas tabelas. Ou seja, só exibe os dados onde há correspondência entre `A.key` e `B.key` .

---
## LEFT JOIN

```sql
SELECT *  
FROM A  
LEFT JOIN B ON A.key = B.key
```
  Explicação: Retorna todos os registros da tabela A , e os registros correspondentes da tabela B . Se não houver correspondência em B , os valores de B serão NULL .
### LEFT JOIN com Verificação de NULL
 	
```sql
SELECT *  
FROM A  
LEFT JOIN B ON A.key = B.key  
WHERE B.key IS NULL
```
  Explicação: Retorna todos os registros da tabela A onde não há correspondência na tabela B . Útil para encontrar registros na tabela A que não possuem equivalente na tabela B .

---
## RIGHT JOIN
 
```sql
SELECT *  
FROM A  
RIGHT JOIN B ON A.key = B.key
```
  Explicação: Retorna todos os registros da tabela B , e os registros correspondentes da tabela A . Se não houver correspondência em A , os valores de A serão NULL .
### RIGHT JOIN com Verificação de NULL

```sql
SELECT *  
FROM A  
RIGHT JOIN B ON A.key = B.key  
WHERE A.key IS NULL
```
  Explicação: Retorna todos os registros da tabela B onde não há correspondência na tabela A . Útil para encontrar registros na tabela B que não possuem equivalente na tabela A .  

---
## FULL JOIN
 	
```sql
SELECT *  
FROM A  
FULL OUTER JOIN B ON A.key = B.key
```
  Explicação: Retorna todos os registros quando há uma correspondência em qualquer das tabelas, ou seja, combina os resultados de LEFT JOIN e RIGHT JOIN .  
### FULL JOIN com Verificação de NULL
 	
```sql
SELECT *  
FROM A  
FULL OUTER JOIN B ON A.key = B.key  
WHERE A.key IS NULL OR B.key IS NULL
```
Explicação: Retorna registros de ambas as tabelas onde não há correspondência na outra tabela. Útil para encontrar registros únicos em A ou B .

---
Essas operações são fundamentais para combinar dados de diferentes tabelas em bancos de dados relacionais, cada uma sendo adequada para diferentes necessidades de análise de dados.

