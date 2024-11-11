---
tags:
  - TI/SMART
  - TI/Tecnologia/Prometheus/Node-Exporter
---
Aqui está uma lista detalhada de métricas que você pode usar para monitorar o desempenho e a saúde do disco com o `node_exporter` no Grafana. Essas métricas são essenciais para entender a operação do seu sistema de armazenamento e identificar possíveis problemas.

### 1. `node_disk_read_bytes_total`
- **Descrição**: Total de bytes lidos a partir de um disco específico desde o início do monitoramento.
- **Utilização**: Essa métrica é útil para monitorar a quantidade de dados que estão sendo lidos do disco. Um aumento constante pode indicar um alto uso do disco para leituras, enquanto quedas podem sinalizar problemas.

### 2. `node_disk_written_bytes_total`
- **Descrição**: Total de bytes escritos em um disco específico desde o início do monitoramento.
- **Utilização**: Essa métrica é importante para avaliar o volume de dados gravados no disco. Um aumento pode indicar um grande número de operações de gravação, enquanto uma diminuição ou estabilização pode sugerir que menos dados estão sendo gravados.

### 3. `node_disk_read_time_seconds_total`
- **Descrição**: Tempo total gasto em operações de leitura de um disco em segundos.
- **Utilização**: Essa métrica pode ajudar a medir a eficiência das operações de leitura. Um tempo de leitura elevado pode indicar que o disco está enfrentando congestionamentos ou problemas de desempenho.

### 4. `node_disk_write_time_seconds_total`
- **Descrição**: Tempo total gasto em operações de gravação de um disco em segundos.
- **Utilização**: Assim como o tempo de leitura, o tempo de gravação elevado pode sugerir que o disco está sobrecarregado ou que há problemas que afetam a taxa de gravação.

### 5. `node_disk_io_time_seconds_total`
- **Descrição**: Tempo total gasto em operações de I/O (entrada/saída) de disco.
- **Utilização**: Essa métrica fornece uma visão geral do tempo que o disco está ocupado realizando operações de I/O. Um valor elevado pode indicar que o disco está constantemente ocupado, o que pode afetar o desempenho do sistema.

### 6. `node_disk_io_time_weighted_seconds_total`
- **Descrição**: Tempo total de I/O ponderado, que considera a latência das operações de I/O.
- **Utilização**: Essa métrica é útil para avaliar a latência das operações. Um tempo ponderado alto pode indicar que muitas operações estão levando tempo excessivo.

### 7. `node_disk_reads_completed_total`
- **Descrição**: Total de operações de leitura concluídas no disco.
- **Utilização**: Esse número ajuda a monitorar o número total de leituras realizadas. É útil para entender a carga de trabalho de leitura em um disco.

### 8. `node_disk_writes_completed_total`
- **Descrição**: Total de operações de gravação concluídas no disco.
- **Utilização**: Essa métrica monitora quantas gravações foram realizadas, ajudando a entender a carga de trabalho de gravação.

### 9. `node_disk_partitioned`
- **Descrição**: Indica se o disco foi particionado.
- **Utilização**: Essa métrica é útil para verificar a estrutura do disco, ajudando a entender como os discos estão organizados em partições.

### 10. `node_disk_saturation`
- **Descrição**: Medida de quão saturado o disco está em relação ao que pode processar.
- **Utilização**: Essa métrica pode indicar se o disco está se aproximando de seus limites máximos, o que pode impactar o desempenho do sistema.

### Resumo

Essas métricas ajudam a monitorar não apenas a performance dos discos, mas também a saúde geral do sistema de armazenamento. Você pode usar essas informações no Grafana para criar dashboards que visualizam o uso, a eficiência e a saúde dos seus discos, permitindo uma análise mais fácil e rápida para detectar e diagnosticar problemas.

### Referências

- Documentação do Node Exporter: https://github.com/prometheus/node_exporter
- Artigo sobre monitoramento de disco com Prometheus: https://prometheus.io/docs/introduction/overview/
