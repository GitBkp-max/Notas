---
tags:
  - TI/SMART
  - TI/Tecnologia/Prometheus/Node-Exporter
  - TI/Tecnologia/Grafana
---
Para configurar a coleta de dados de disco no Grafana em um servidor Linux "cru", vamos instalar e configurar o Prometheus e o Node Exporter, que são ferramentas comuns para monitoramento de sistema, e configurá-los para enviar dados ao Grafana. Vou te guiar do começo ao fim com todos os passos.

### 1. **Instalar o Prometheus**
Primeiro, você precisará instalar o [[Prometheus]], que será o servidor de monitoramento para coletar e armazenar as métricas.

#### Passo 1.1: Baixar e instalar o Prometheus

```bash
cd /opt
sudo wget https://github.com/prometheus/prometheus/releases/download/v2.47.0/prometheus-2.47.0.linux-amd64.tar.gz
sudo tar xvf prometheus-2.47.0.linux-amd64.tar.gz
sudo mv prometheus-2.47.0.linux-amd64 prometheus
```

#### Passo 1.2: Configurar o Prometheus
Crie um arquivo de configuração para o Prometheus (`/opt/prometheus/prometheus.yml`). Este arquivo deve incluir a configuração para coletar dados do **Node Exporter** (que instalaremos a seguir).

Exemplo básico de configuração (`/opt/prometheus/prometheus.yml`):

```yaml
global:
  scrape_interval: 15s  # Coleta de dados a cada 15 segundos

scrape_configs:
  - job_name: 'node_exporter'
    static_configs:
      - targets: ['localhost:9100']  # Node Exporter será executado na porta 9100
```

#### Passo 1.3: Iniciar o Prometheus
Crie um serviço systemd para gerenciar o Prometheus.

Crie o arquivo `/etc/systemd/system/prometheus.service`:

```ini
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=root
ExecStart=/opt/prometheus/prometheus --config.file=/opt/prometheus/prometheus.yml
Restart=always

[Install]
WantedBy=multi-user.target
```

Agora, habilite e inicie o Prometheus:

```bash
sudo systemctl daemon-reload
sudo systemctl enable prometheus
sudo systemctl start prometheus
```

O Prometheus estará acessível via o endereço `http://localhost:9090`.

### 2. **Instalar o Node Exporter**
O [[Prometheus#Node-Exporter|Node Exporter]] coleta métricas de sistema, como uso de CPU, memória e disco, que são enviadas para o Prometheus.

#### Passo 2.1: Baixar e instalar o Node Exporter

```bash
cd /opt
sudo wget https://github.com/prometheus/node_exporter/releases/download/v1.7.0/node_exporter-1.7.0.linux-amd64.tar.gz
sudo tar xvf node_exporter-1.7.0.linux-amd64.tar.gz
sudo mv node_exporter-1.7.0.linux-amd64 node_exporter
```

#### Passo 2.2: Iniciar o Node Exporter
Crie um serviço systemd para o Node Exporter:

Crie o arquivo `/etc/systemd/system/node_exporter.service`:

```ini
[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=root
ExecStart=/opt/node_exporter/node_exporter
Restart=always

[Install]
WantedBy=multi-user.target
```

Habilite e inicie o Node Exporter:

```bash
sudo systemctl daemon-reload
sudo systemctl enable node_exporter
sudo systemctl start node_exporter
```

O **Node Exporter** agora estará rodando na porta 9100 (verifique com `http://localhost:9100/metrics`).

### 3. **Configurar o Grafana**
Agora que o Prometheus está rodando e coletando métricas do Node Exporter, vamos configurar o [[Grafana]] para exibir essas métricas.

#### Passo 3.1: Adicionar o Prometheus como uma fonte de dados no Grafana

1. Acesse o Grafana (`http://localhost:3000`) e faça login.
2. Vá para o menu **Configuration** (ícone de engrenagem) e clique em **Data Sources**.
3. Clique no botão **Add data source**.
4. Escolha **Prometheus**.
5. No campo **URL**, insira `http://localhost:9090`.
6. Clique em **Save & Test** para verificar se a conexão está funcionando.

#### Passo 3.2: Criar um dashboard para monitorar o uso de disco

1. No menu principal do Grafana, clique em **+ Create** e escolha **Dashboard**.
2. Adicione um **novo painel** clicando em **Add new panel**.
3. No campo de consulta, insira uma das consultas para monitorar o uso de disco:

   - **Espaço livre em disco**:
     ```promql
     node_filesystem_avail_bytes{fstype!="tmpfs", fstype!="devtmpfs"}
     ```

   - **Uso total de disco**:
     ```promql
     node_filesystem_size_bytes{fstype!="tmpfs", fstype!="devtmpfs"}
     ```

   - **Percentual de uso de disco**:
     ```promql
     (node_filesystem_size_bytes{fstype!="tmpfs", fstype!="devtmpfs"} - node_filesystem_avail_bytes{fstype!="tmpfs", fstype!="devtmpfs"}) / node_filesystem_size_bytes{fstype!="tmpfs", fstype!="devtmpfs"} * 100
     ```

4. Customize o gráfico e clique em **Save** para salvar o painel.

### 4. **Configurar alertas (opcional)**
Se você quiser configurar alertas para quando o disco estiver quase cheio, faça isso dentro do painel de monitoramento que você criou no passo anterior. No painel de consulta, adicione uma regra de alerta para quando o uso de disco ultrapassar um determinado limite, como 80%.

### 5. **Finalização**
Agora você tem um sistema de monitoramento básico configurado com Prometheus, Node Exporter e Grafana para monitorar o uso de disco.

Se você quiser adicionar mais hosts para monitorar, basta repetir os passos para instalar o Node Exporter em cada servidor e adicioná-los ao arquivo de configuração do Prometheus.

---

**Referências**:
- Prometheus: https://prometheus.io/docs/introduction/overview/
- Node Exporter: https://prometheus.io/docs/guides/node-exporter/
- Grafana: https://grafana.com/docs/grafana/latest/getting-started/getting-started-prometheus/

**Mais Detalhes de monitoramento**
- [[monitorar o estado do disco (incluindo capacidade, utilização e saúde)]]