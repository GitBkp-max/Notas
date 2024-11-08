---
tags:
  - Tecnologia/DBA
---
Para inverter o processo e exibir apenas os registros onde `b.incident_status_id` é igual a 4 ou 8, você deve substituir a condição <span style="color:rgb(0, 112, 192)">NOT IN</span> por <span style="color:rgb(0, 112, 192)">IN</span>. A linha modificada ficará assim: 

```sql
b.incident_status_id IN (4, 8)
```

A consulta completa ficaria assim:

```sql
SELECT a.id, d.title AS "Tipo", p.v_name AS "Nome", c.title AS "Equipe", a.title AS "Titulo", description AS "Descricao", a.created "Criacao"

FROM assignments a 
JOIN erp.assignment_incidents b ON a.id = b.assignment_id
JOIN erp.people p ON p.id = a.responsible_id
JOIN erp.teams c ON c.id = b.team_id
JOIN erp.incident_types d ON d.id = b.incident_type_id

WHERE
b.incident_status_id IN (4, 8)
AND a.progress < 100
AND a.deleted = FALSE
AND a.created BETWEEN $__timeFrom() AND $__timeTo() 
ORDER BY a.created DESC
```

Isso irá exibir apenas os registros onde ``b.incident_status_id`` é 4 ou 8. 

Referência: Conceitos básicos de SQL, disponíveis em tutoriais SQL como o do 
W3Schools - https://www.w3schools.com/sql/sql_in.asp
