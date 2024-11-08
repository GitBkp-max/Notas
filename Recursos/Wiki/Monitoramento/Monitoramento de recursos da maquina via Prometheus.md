---
tags:
  - Prometheus/Node-Exporter
  - Prometheus
  - Grafana
---
Para visualizar o uso de memória, rede e processador de uma máquina no Grafana via Prometheus, siga estas etapas:
### 1. **Configuração do Prometheus**
Primeiro, você precisa garantir que o Prometheus está coletando as métricas corretas da máquina que deseja monitorar. Para isso, utilize o **Node Exporter**, que é um exportador oficial para métricas de sistemas Linux.

#### Passos:
- Instale o Node Exporter na máquina que deseja monitorar:
  ```bash
  wget https://github.com/prometheus/node_exporter/releases/download/v1.6.1/node_exporter-1.6.1.linux-amd64.tar.gz
  tar xvfz node_exporter-1.6.1.linux-amd64.tar.gz
  cd node_exporter-1.6.1.linux-amd64
  ./node_exporter
  ```

- Configure o Prometheus para coletar as métricas do Node Exporter. Adicione o seguinte no arquivo `prometheus.yml`:
  ```yaml
  scrape_configs:
    - job_name: 'node_exporter'
      static_configs:
        - targets: ['localhost:9100']
  ```

### 2. **Configuração no Grafana**
Com o Prometheus coletando as métricas, agora vamos configurar os dashboards no Grafana para visualizar o uso de CPU, memória e rede.

#### Passos:
1. **Adicione o Prometheus como uma fonte de dados no Grafana**:
   - Vá até a página de configuração de fontes de dados no Grafana.
   - Escolha Prometheus como o tipo de fonte de dados e insira a URL do seu Prometheus, geralmente `http://localhost:9090`.

2. **Crie um Dashboard**:
   - Vá para "Create" > "Dashboard" no Grafana.
   - Adicione novos "Panels" para cada métrica.

3. **Consultas PromQL**:
   Use as seguintes consultas para exibir as métricas:

   - **Uso de CPU**:
     ```promql
     100 - (avg by(instance) (irate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)
     ```
     Isso mostra a utilização média da CPU da máquina.

   - **Uso de Memória**:
     ```promql
     node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes * 100
     ```
     Isso exibe a porcentagem de memória disponível.

   - **Tráfego de Rede**:
     - Entrada:
       ```promql
       irate(node_network_receive_bytes_total[5m])
       ```
     - Saída:
       ```promql
       irate(node_network_transmit_bytes_total[5m])
       ```

4. **Ajuste os Gráficos**:
   Personalize a exibição dos gráficos (linhas, barras, gauge, etc.) de acordo com suas preferências, e ajuste os intervalos de tempo como necessário.

### 3. **Dashboards Prontos**:
Se preferir, pode importar dashboards prontos do Grafana Labs, como:
- [Node Exporter Full Dashboard](https://grafana.com/grafana/dashboards/1860-node-exporter-full/).

Basta ir em "Dashboards" > "Import" e colar o ID do dashboard.

### Referências:
- Prometheus: https://prometheus.io/
- Grafana: https://grafana.com/
- Node Exporter: https://github.com/prometheus/node_exporter