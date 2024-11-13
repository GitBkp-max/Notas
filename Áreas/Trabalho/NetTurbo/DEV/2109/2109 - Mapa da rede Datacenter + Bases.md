---
tags:
  - Trabalho/NetTurbo
  - TI/Tecnologia/Zabbix
  - TI/Tecnologia/Grafana
---
```dataview
table completed, date, startTime, endTime
from #Calendario/Trabalho/Atividades 
where contains(file.name, "2109")
sort date asc, startTime asc
```


Data Solicitação: 25/03/2024
Data Coleta de informações: 25/03/2024
Data Aprovação Plano:
Data Inicio Execução: 22/10/2024
Data Termino Execução:

Necessidades: Visualização da comunicação dos equipamentos

Motivo: Facilidade em visualizar o estado da nossa rede
