---
tags:
  - Grafana
  - Tecnologia/DBA
---
Para integrar o filtro de tempo do Grafana na sua consulta SQL, você precisa usar as variáveis de intervalo de tempo do Grafana, como ``$__timeFrom`` e ``$__timeTo``. Atualize sua cláusula WHERE para incluir essas variáveis. Aqui está um exemplo de como pode ficar:

```sql
SELECT a.id, d.title AS "Tipo", p.v_name AS "Nome", c.title AS "Equipe", a.title AS "Titulo", description AS "Descricao", a.created "Criacao"

FROM assignments a 
JOIN erp.assignment_incidents b ON a.id = b.assignment_id
JOIN erp.people p ON p.id = a.responsible_id
JOIN erp.teams c ON c.id = b.team_id
JOIN erp.incident_types d ON d.id = b.incident_type_id
WHERE
b.incident_status_id NOT IN (4, 8)
AND a.progress < 100
AND a.deleted = FALSE
AND a.created BETWEEN $__timeFrom() AND $__timeTo() 
ORDER BY a.created DESC
```

Esse ajuste deve permitir que o filtro de tempo do Grafana funcione corretamente com sua consulta SQL.

A integração das variáveis de intervalo de tempo do Grafana com SQL foi baseada na documentação oficial do Grafana sobre variáveis de intervalo de tempo em consultas SQL. 

Para mais informações, você pode consultar a documentação do Grafana em:
https://grafana.com/docs/grafana/latest/datasources/mysql/#using-macros