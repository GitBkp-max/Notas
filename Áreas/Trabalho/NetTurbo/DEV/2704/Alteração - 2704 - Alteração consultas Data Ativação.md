---
tags:
  - TI/Tecnologia/Grafana
  - Trabalho/NetTurbo/Voalle
  - Trabalho/NetTurbo
  - TI/Tecnologia/DBA/MySQL
---
```dataview
table completed, date, startTime, endTime
from #Calendario/Trabalho/Atividades 
where contains(file.name, "2704")
sort date asc, startTime asc
```
# Alteração - 2704 - Alteração consultas Data Ativação

Data Solicitação: 07/11/2024  
Data Coleta de Requisitos:  13/11/2024
Data Inicio Desenvolvimento:  12/11/2024
Data Termino Desenvolvimento:

Solicitante: Rebeca  

Motivo: 
- Alterar consulta / adicionar campo contendo a data de ativação para melhor analise de contratos  

Necessidade: 
- [x] Levantar quais dash's serão feitas as alterações e realizar a alteração abaixo

> [!NOTE]
> <span style="color:rgb(0, 176, 240)">[10:06, 07/11/2024] </span> Otávio Rigue Voalle Dev:
> Data de ativação pode ser obtida através desta consulta:
> 
> ```sql
> SELECT
> 	c.id,
> 	c."date",
> 	c.approval_date,
> 	c.v_stage,
> 	c.v_status,
> 	c.created,
> 	caa.activation_date
> FROM erp.contracts c
> LEFT JOIN erp.contract_assignment_activations AS caa ON caa.contract_id = c.id
> WHERE 
> 	c.erp_code IS NULL
> 	AND c.stage = 3
> ```
> <span style="color:rgb(0, 176, 240)">[10:08, 07/11/2024] </span> Otávio Rigue Voalle Dev:
> Vale ressaltar q alguns contratos ==não terão esta informação:==
>- Contratos migrados
>- Contratos rejeitados durante a etapa de ativação
>- Alguns casos de cortesia
>- Contratos que não possuam data de aprovação


![[Modelo do relatorio.webp]]

***




