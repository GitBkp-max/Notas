---
tags:
  - Trabalho/NetTurbo
  - TI/Tecnologia/Zabbix
  - TI/SMART
---
Data Solicitação: 01/07/2024
Data Coleta de informações: 01/07/2024
Data Aprovação Plano:
Data Inicio Execução:
Data Termino Execução:

---
Necessidades: 
	Monitoramento do estado dos discos
Motivo: 
	Visualização da vida útil dos discos

---
Template ja pronto: 

```bash
# Para uso desse template é necessario realizar a configuração da tarefa cron e do snmpd

mkdir /var/lib/snmp/output_files
crontab -e 
# Carrega todos os estados de servicos em um arquivo para coleta do zabbix
*/1 * * * *  systemctl list-units --type=service --all --no-pager --no-legend --plain --output=json > /var/lib/snmp/output_files/systemctl.json

# adicionar linha no /etc/snmp/snmpd.conf

# retorna o conteudo do estado do systemctl que eh coletado pelo cron
extend systemctl_state /usr/bin/cat /var/lib/snmp/output_files/systemctl.json
```

[[Explicação do template Zabbix|Explicação desse template feito pelo GPT]]