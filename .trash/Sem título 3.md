Vamos refazer tudo considerando uma instalação do Loki sem o Docker, diretamente no sistema.

### 1 - Como instalar o Loki (sem Docker)

**Passos para instalação em um sistema Linux:**

1. **Baixe o Loki:**
   - Você pode baixar o binário diretamente do GitHub:
     ```bash
     wget https://github.com/grafana/loki/releases/download/v2.8.2/loki-linux-amd64.zip
     unzip loki-linux-amd64.zip
     chmod a+x loki-linux-amd64
     sudo mv loki-linux-amd64 /usr/local/bin/loki
     ```

2. **Crie o arquivo de configuração:**
   - Crie o diretório de configuração:
     ```bash
     sudo mkdir -p /etc/loki
     ```

   - Crie um arquivo chamado `loki-config.yaml` dentro desse diretório:
     ```yaml
     auth_enabled: false
     server:
       http_listen_port: 3100
     ingester:
       lifecycler:
         ring:
           kvstore:
             store: inmemory
           replication_factor: 1
         final_sleep: 0s
       chunk_idle_period: 3m
       chunk_retain_period: 1m
       max_transfer_retries: 0
     schema_config:
       configs:
         - from: 2020-10-24
           store: boltdb-shipper
           object_store: filesystem
           schema: v11
           index:
             prefix: index_
             period: 24h
     storage_config:
       boltdb_shipper:
         active_index_directory: /tmp/loki/boltdb-shipper-active
         cache_location: /tmp/loki/boltdb-shipper-cache
         shared_store: filesystem
       filesystem:
         directory: /tmp/loki/chunks
     limits_config:
       enforce_metric_name: false
       reject_old_samples: true
       reject_old_samples_max_age: 168h
     ```

3. **Inicie o Loki:**
   - Você pode iniciar o Loki manualmente com o seguinte comando:
     ```bash
     loki -config.file=/etc/loki/loki-config.yaml
     ```

   - Para rodá-lo em segundo plano como um serviço, crie um arquivo de serviço systemd:
     ```bash
     sudo nano /etc/systemd/system/loki.service
     ```

     Insira o seguinte conteúdo:
     ```ini
     [Unit]
     Description=Loki Log Aggregator
     After=network.target

     [Service]
     ExecStart=/usr/local/bin/loki -config.file=/etc/loki/loki-config.yaml
     Restart=on-failure

     [Install]
     WantedBy=multi-user.target
     ```

     Em seguida, inicie e habilite o serviço:
     ```bash
     sudo systemctl daemon-reload
     sudo systemctl start loki
     sudo systemctl enable loki
     ````
### 2 - Como configurar (resolvendo problema do Loki não coletar dados no Grafana)

Se o Loki já está instalado, mas não coleta dados no Grafana, siga estes passos:

1. **Verifique a conexão entre Grafana e Loki:**
   - No Grafana, vá até **Configuration → Data Sources** e selecione **Loki**.
   - Verifique se a URL está correta, como `http://localhost:3100` (ou o IP correto se rodar remotamente).
   - Clique em **Save & Test**. Se a conexão falhar, revise o arquivo `loki-config.yaml` e veja se o Loki está ativo com:
     ```bash
     sudo systemctl status loki
     ```

2. **Verifique se o Loki está recebendo logs:**
   - Utilize o comando curl para verificar se o Loki está recebendo métricas:
     ```bash
     curl http://localhost:3100/metrics
     ```

3. **Configure o Promtail (agente de logs):**
   - O Promtail coleta logs do sistema e envia para o Loki. Instale o Promtail:
     ```bash
     wget https://github.com/grafana/loki/releases/download/v2.8.2/promtail-linux-amd64.zip
     unzip promtail-linux-amd64.zip
     chmod a+x promtail-linux-amd64
     sudo mv promtail-linux-amd64 /usr/local/bin/promtail
     ```

   - Crie um arquivo de configuração para o Promtail:
     ```bash
     sudo nano /etc/promtail/promtail-config.yaml
     ```

     Um exemplo básico de configuração:
     ```yaml
     server:
       http_listen_port: 9080
       grpc_listen_port: 0

     positions:
       filename: /tmp/positions.yaml

     clients:
       - url: http://localhost:3100/loki/api/v1/push

     scrape_configs:
       - job_name: system
         static_configs:
           - targets:
               - localhost
             labels:
               job: varlogs
               __path__: /var/log/*log
     ```

   - Inicie o Promtail como um serviço:
     ```bash
     sudo systemctl start promtail
     sudo systemctl enable promtail
     ```

4. **Verifique os logs do Loki e do Promtail:**
   - Se ainda houver problemas, verifique os logs dos serviços:
     ```bash
     sudo journalctl -u loki
     sudo journalctl -u promtail
     ```

### 3 - Como conectar ao Grafana

A conexão entre o Loki e o Grafana é feita configurando o Loki como uma fonte de dados no Grafana:

1. Acesse **Configuration → Data Sources** no Grafana.
2. Clique em **Add Data Source** e escolha **Loki**.
3. Insira a URL do Loki (por exemplo, `http://localhost:3100`).
4. Teste a conexão e salve.

### 4 - Tudo sobre o Loki

O Loki é uma ferramenta de agregação de logs projetada para ser eficiente, sem indexar o conteúdo dos logs, o que o torna mais leve que outras soluções de log centralizado. Ele é parte do stack de monitoramento da Grafana, junto com o Grafana para visualização e o Prometheus para métricas.

Componentes principais:
- **Loki**: Agregador de logs.
- **Promtail**: Coletor de logs que envia para o Loki.
- **Grafana**: Interface de visualização.

### 5 - Exemplos de monitoramento com Loki

- **Consulta básica para logs de erros:**
  ```logql
  {job="varlogs"} |= "error"
  ```
  Isso filtra logs do job `varlogs` e retorna apenas os que contêm a palavra "error".

- **Contagem de logs em um período de tempo:**
  ```logql
  count_over_time({job="varlogs"}[5m])
  ```
  Conta a quantidade de logs nos últimos 5 minutos.

- **Filtro por status HTTP:**
  ```logql
  {job="nginx"} |= "404"
  ```
  Busca logs de um servidor Nginx com status 404.

### 6 - Curiosidades e tipos de monitoramento com Loki

- **Escalabilidade:** Loki pode ser utilizado em arquiteturas distribuídas, sendo escalável para grandes volumes de logs.
- **Flexibilidade com LogQL:** Com a linguagem LogQL, é possível criar consultas avançadas sobre os logs, como agregar, contar e filtrar dados.
- **Integração fácil com Kubernetes:** Loki é especialmente útil para monitoramento de clusters Kubernetes, onde cada pod gera seus próprios logs.

Se precisar de mais exemplos ou ajustes específicos, é só avisar!

**Referências:**
- https://grafana.com/docs/loki/latest/
- https://github.com/grafana/loki