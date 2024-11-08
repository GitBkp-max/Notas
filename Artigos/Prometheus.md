---
tags:
  - Prometheus
  - Grafana
  - Artigos
---
## Sobre o Prometheus

O **Prometheus** é uma ferramenta de monitoramento e alerta voltada para sistemas e serviços. É popular por ser altamente escalável, eficiente e por sua capacidade de monitorar métricas em tempo real. Desenvolvido pela SoundCloud e posteriormente doado à **Cloud Native Computing Foundation** (CNCF), o Prometheus se tornou uma solução de monitoramento de referência, especialmente em ambientes **cloud-native** e arquiteturas baseadas em **containers** (como Kubernetes).

### Principais características do Prometheus:
1. **Modelo de dados baseado em séries temporais**: 
   - O Prometheus coleta métricas e armazena essas informações como séries temporais (valores com timestamp), permitindo consultas e visualizações com base em intervalos de tempo.
   
2. **Pull-based scraping**:
   - O Prometheus coleta métricas a partir de endpoints HTTP que expõem dados em um formato específico (usualmente JSON ou texto simples). Ao invés de o agente enviar os dados, o Prometheus faz a solicitação (pull) periodicamente.

3. **Consulta via PromQL**:
   - A linguagem de consulta do Prometheus, chamada **PromQL**, permite a extração de métricas e análise sofisticada dos dados coletados. É flexível e poderosa, possibilitando calcular taxas, médias, somas e outras operações com as métricas.

4. **Alertas e integração com Alertmanager**:
   - O Prometheus permite a configuração de alertas com base em regras definidas no PromQL. Para o envio e gerenciamento dos alertas, ele é geralmente integrado ao **Alertmanager**, que pode encaminhá-los para canais como e-mail, Slack, ou outras plataformas.

5. **Alta disponibilidade e escalabilidade**:
   - Apesar de ser eficiente para monitorar grandes volumes de dados, o Prometheus foi projetado para operar como uma solução autônoma. Ele pode ser replicado para alta disponibilidade e escalabilidade, mas cada instância armazena seus próprios dados localmente.

6. **Facilidade de integração**:
   - O Prometheus possui uma vasta gama de **exporters**, que são programas responsáveis por coletar métricas de sistemas, como bancos de dados (MySQL, PostgreSQL), servidores de aplicação, sistemas operacionais, hardware, etc. Isso facilita a integração do Prometheus com diversos serviços e infraestruturas.

7. **Dashboarding**:
   - Embora o Prometheus não tenha uma interface de visualização muito avançada por si só, ele é frequentemente usado em conjunto com ferramentas como o **Grafana** para a criação de dashboards ricos e interativos.

### Exemplo de uso para monitoramento de disco
Para monitorar o uso de disco, você pode usar o **node_exporter**, que expõe diversas métricas relacionadas ao sistema, como uso de CPU, memória e disco.

1. Instale o **node_exporter** no servidor.
2. Configure o Prometheus para fazer o scrape das métricas do node_exporter.
3. A partir daí, você pode escrever consultas PromQL para monitorar o uso de disco. Exemplo de uma consulta básica:

```promql
node_filesystem_avail_bytes{fstype!="tmpfs"} / node_filesystem_size_bytes{fstype!="tmpfs"}
```

Essa consulta mostra o percentual de espaço livre no sistema de arquivos, excluindo os sistemas de arquivos temporários (tmpfs).

Mais tipos de [[monitorar o estado do disco (incluindo capacidade, utilização e saúde)|monitoramento de disco]]

### Referências:
- **Site oficial**: https://prometheus.io/
- **Documentação completa**: https://prometheus.io/docs/
- **Exporters para diversas integrações**: https://prometheus.io/docs/instrumenting/exporters/
- **Dashboard de exemplo no Grafana**: https://grafana.com/grafana/dashboards

## Node-Exporter

O #Prometheus/Node-Exporter node_exporter é um dos **exporters** mais usados no ecossistema Prometheus. Ele é um agente de monitoramento especializado em coletar métricas relacionadas ao sistema operacional e expô-las no formato esperado pelo Prometheus, permitindo que você monitore os recursos físicos e de sistema de máquinas Linux (e parcialmente, máquinas Windows).

### Características principais do node_exporter:
1. **Coleta de métricas do sistema**:
   - O node_exporter coleta diversas métricas do sistema, como:
     - **Uso de CPU** (carga, tempo de uso por core)
     - **Uso de memória** (total, disponível, buffers, swap)
     - **Uso de disco** (espaço utilizado, espaço disponível, I/O de disco)
     - **Estatísticas de rede** (taxa de pacotes, erros, conexões TCP)
     - **Temperatura do hardware** (se suportado)
     - **Estatísticas de processos** (quantidade de processos e threads)
   
2. **Extensível**:
   - O node_exporter permite a habilitação de diversos **coletores de métricas** (collectors). Por padrão, ele coleta uma boa quantidade de métricas, mas é possível configurar para coletar métricas adicionais, dependendo das necessidades. Você pode ativar ou desativar coletores específicos conforme necessário.

3. **Fácil de usar**:
   - O node_exporter é uma aplicação de linha de comando simples que pode ser iniciada como um serviço no sistema. Uma vez rodando, ele abre uma porta HTTP (usualmente a porta 9100) onde as métricas ficam disponíveis para o Prometheus fazer o **scrape**.

4. **Monitoramento sem agentes complexos**:
   - Ao contrário de algumas soluções de monitoramento que exigem a instalação de agentes mais complexos, o node_exporter é leve e usa pouca memória e CPU. Ele é voltado para exposição de métricas de forma passiva.

5. **Integração com Prometheus**:
   - O Prometheus se comunica com o node_exporter via o padrão **HTTP pull**, ou seja, o Prometheus faz requisições HTTP periódicas ao node_exporter para obter as métricas expostas. A configuração do Prometheus para monitorar um node_exporter é simples, bastando adicionar o endpoint no arquivo de configuração do Prometheus.

### Exemplo de métricas coletadas:
Quando você acessa o node_exporter via `http://seu_servidor:9100/metrics`, ele expõe um grande volume de dados sobre o sistema. Aqui estão algumas das métricas mais comuns que ele fornece:

- **CPU**:
  - `node_cpu_seconds_total`: Tempo total de CPU gasto em diversos estados (user, system, idle, iowait, etc.)
  
- **Memória**:
  - `node_memory_MemTotal_bytes`: Quantidade total de memória disponível no sistema.
  - `node_memory_MemAvailable_bytes`: Memória disponível para novos processos.

- **Disco**:
  - `node_filesystem_size_bytes`: Tamanho total de um filesystem.
  - `node_filesystem_free_bytes`: Quantidade de espaço livre no disco.
  - `node_disk_io_time_seconds_total`: Tempo gasto em operações de I/O de disco.

- **Rede**:
  - `node_network_receive_bytes_total`: Bytes recebidos em todas as interfaces de rede.
  - `node_network_transmit_bytes_total`: Bytes transmitidos em todas as interfaces de rede.

### Instalação e configuração básica:
1. **Instalação**:
   - Em sistemas baseados em Debian/Ubuntu:
```bash
sudo apt-get update
sudo apt-get install prometheus-node-exporter
```
   - Em sistemas baseados em RedHat/CentOS:
```bash
sudo yum install node_exporter
```

2. **Execução**:
   - Depois de instalado, basta executar o node_exporter:
```bash
./node_exporter
```
   - Isso iniciará o node_exporter e ele estará escutando na porta 9100, por padrão.

3. **Configuração no Prometheus**:
   - No arquivo de configuração do Prometheus (`prometheus.yml`), adicione o seguinte para monitorar o node_exporter:
     ```yaml
     scrape_configs:
       - job_name: 'node_exporter'
         static_configs:
           - targets: ['localhost:9100']  # ou o IP do servidor
     ```

4. **Dashboard com Grafana**:
   - Se você estiver usando o Grafana para visualização, há dashboards prontos na comunidade Grafana para monitorar métricas do node_exporter. Basta importar o dashboard ID **1860** do Grafana Labs.

### Uso prático: monitoramento de disco com node_exporter
Uma das principais funcionalidades do node_exporter é o monitoramento de disco. Usando o node_exporter, você pode obter métricas como espaço livre, total, uso de I/O, entre outras. Por exemplo:

- Para visualizar o espaço disponível no disco, você pode usar uma consulta PromQL como esta:
```promql
node_filesystem_avail_bytes / node_filesystem_size_bytes * 100
```

  Isso calculará a porcentagem de espaço livre em cada sistema de arquivos monitorado.

> [!caution] Aviso
> o **node_exporter** não tem nada a ver com o **Node.js**. O **node_exporter** é uma ferramenta para coletar métricas do sistema (como CPU, memória e disco) para o **Prometheus**, enquanto o **Node.js** é uma plataforma de execução de JavaScript no lado do servidor.

### Referências:
- Site oficial: https://prometheus.io/
- Documentação do node_exporter: https://github.com/prometheus/node_exporter
- Dashboard de Grafana para node_exporter: https://grafana.com/grafana/dashboards

## Smartctl-Exporter

#prometheus/Smartctl-exporter 

O **smartctl_exporter** é uma ferramenta que coleta e exporta métricas do sistema de monitoramento de saúde de discos rígidos e SSDs usando o comando `smartctl`, que faz parte do pacote `smartmontools`. Essa ferramenta é comumente usada em ambientes de monitoramento, como o Prometheus, para rastrear a saúde e o desempenho dos dispositivos de armazenamento.

### Principais Recursos

1. **Coleta de Métricas**:
   - Coleta informações S.M.A.R.T. (Self-Monitoring, Analysis, and Reporting Technology) dos dispositivos de armazenamento.
   - Exporta métricas como temperatura, tempo de operação, número de erros de leitura/gravação, contagem de setores re-alocados, entre outros.

2. **Compatibilidade**:
   - Funciona com diversos tipos de dispositivos de armazenamento, incluindo HDDs e SSDs, de diferentes fabricantes.

3. **Monitoramento em Tempo Real**:
   - Permite monitorar a saúde dos dispositivos em tempo real, ajudando na detecção precoce de falhas.

4. **Integração com Prometheus**:
   - Facilita a visualização de métricas através de ferramentas de dashboard, como Grafana, utilizando o Prometheus como backend.

### Como Usar

Para utilizar o smartctl_exporter, você geralmente precisará seguir estas etapas:

1. **Instalação**:
   - Pode ser instalado via pacotes ou diretamente do repositório do GitHub.
   - Dependências incluem o `smartmontools`.

2. **Configuração**:
   - Pode ser configurado através de um arquivo de configuração, onde você especifica quais dispositivos monitorar.
   - As opções de linha de comando permitem personalizar a coleta e o intervalo de atualização.

3. **Execução**:
   - O exporter deve ser executado em segundo plano, escutando em uma porta específica (por padrão, a porta 9100) para que o Prometheus possa coletar as métricas.

4. **Coleta de Métricas pelo Prometheus**:
   - O Prometheus deve ser configurado para coletar métricas do smartctl_exporter especificando o endpoint que ele está escutando.

### Exemplo de Comando

Um exemplo básico de como iniciar o smartctl_exporter seria:
```bash
./smartctl_exporter --devices /dev/sda,/dev/sdb
```

Isso instruiria o exporter a coletar dados dos dispositivos `/dev/sda` e `/dev/sdb`.

### Referências

- GitHub do smartctl_exporter: https://github.com/Prometheus/smartctl_exporter
- Documentação do smartmontools: https://www.smartmontools.org

Se precisar de mais alguma coisa, é só avisar!
Essas informações devem te ajudar a entender melhor como funciona o smartctl_exporter e como implementá-lo em seu ambiente. Se precisar de mais detalhes ou de um exemplo específico, é só avisar!