---
tags:
  - Grafana
  - Prometheus/Node-Exporter
  - SO/Linux
  - Prometheus
aliases:
  - monitoramento de disco
---
Para monitorar o **estado do disco** (incluindo capacidade, utilização e saúde), vamos focar em algumas métricas principais usando o **[[Prometheus#Node-Exporter|Node Exporter]]** e o **[[Prometheus]]** no **[[Grafana]]**. O **Node Exporter** fornece métricas detalhadas sobre o estado do disco, como espaço disponível, espaço usado e performance I/O.

Aqui estão as métricas principais relacionadas ao **estado do disco** que você pode configurar e visualizar no Grafana:

### 1. **Espaço disponível no disco**
Esta métrica mostra quanto espaço livre está disponível em um disco.

#### Consulta no Prometheus:
```promql
node_filesystem_avail_bytes{fstype!="tmpfs", fstype!="devtmpfs"}
```

Isso mostrará a quantidade de bytes disponíveis em cada sistema de arquivos montado, exceto os sistemas de arquivos temporários (`tmpfs` e `devtmpfs`).

### 2. **Espaço total do disco**
Esta métrica mostra o tamanho total de um disco.

#### Consulta no Prometheus:
```promql
node_filesystem_size_bytes{fstype!="tmpfs", fstype!="devtmpfs"}
```

### 3. **Porcentagem de uso do disco**
Esta métrica calcula a porcentagem de uso do disco com base no espaço total e no espaço disponível.

#### Consulta no Prometheus:
```promql
(node_filesystem_size_bytes{fstype!="tmpfs", fstype!="devtmpfs"} - node_filesystem_avail_bytes{fstype!="tmpfs", fstype!="devtmpfs"}) / node_filesystem_size_bytes{fstype!="tmpfs", fstype!="devtmpfs"} * 100
```

### 4. **Taxa de leitura/escrita do disco (I/O)**
Para monitorar a quantidade de bytes lidos e escritos no disco, você pode usar as seguintes métricas.

#### Leitura de disco:
```promql
rate(node_disk_read_bytes_total[5m])
```

#### Escrita no disco:
```promql
rate(node_disk_written_bytes_total[5m])
```

Essas consultas mostram a quantidade de bytes lidos/escritos por segundo nas últimas 5 minutos (ajustável).

### 5. **Contagem de operações de I/O**
Você pode monitorar o número de operações de leitura/escrita que estão sendo realizadas no disco.

#### Leituras de disco:
```promql
rate(node_disk_reads_completed_total[5m])
```

#### Escritas de disco:
```promql
rate(node_disk_writes_completed_total[5m])
```

### 6. **Latência de I/O**
A latência de operações de I/O mostra o tempo médio que as operações de leitura e escrita levam para serem concluídas.

#### Latência de leitura:
```promql
rate(node_disk_read_time_seconds_total[5m]) / rate(node_disk_reads_completed_total[5m])
```

#### Latência de escrita:
```promql
rate(node_disk_write_time_seconds_total[5m]) / rate(node_disk_writes_completed_total[5m])
```

### 7. **Alertas para estado crítico do disco**
No Grafana, você pode configurar alertas com base nas consultas acima. Por exemplo, para alertar quando o disco atingir 80% de uso:

#### Configuração de alerta no Grafana:
1. No painel com a consulta de porcentagem de uso do disco, vá até a aba **Alerting**.
2. Adicione uma nova regra de alerta com a condição de disparo quando a métrica de uso de disco ultrapassar 80%.
3. Defina o comportamento do alerta (notificação por e-mail, webhook, etc.).

### Considerações finais
Essas métricas fornecem um panorama completo do **estado do disco**. O monitoramento pode ser ajustado para incluir mais detalhes específicos, como uso por partição ou dispositivo.

---

**Referências**:
- Node Exporter: https://prometheus.io/docs/guides/node-exporter/
- Prometheus: https://prometheus.io/docs/prometheus/latest/querying/basics/

[[Metricas de monitoramento e desempenho de maquina|Mais metricas possiveis SMART]]
