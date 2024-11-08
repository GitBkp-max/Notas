---
tags:
  - Grafana
  - SO/Linux
  - Prometheus
---
Para monitorar o estado SMART dos discos em um sistema Linux e exibir as informações no Grafana, você pode seguir os seguintes passos:

## SMART

### 1. Instalação do `smartmontools`

Primeiro, você precisa instalar o pacote `smartmontools`, que fornece as ferramentas para monitoramento SMART.

```bash
sudo apt-get update
sudo apt-get install smartmontools
```

### 2. Configuração do `smartd`

O serviço `smartd`, que faz parte do `smartmontools`, pode ser configurado para monitorar os discos.

- Edite o arquivo de configuração:

```bash
sudo nano /etc/smartd.conf
```

- Adicione linhas para cada disco que deseja monitorar. Por exemplo, para monitorar `/dev/sda` e `/dev/sdb`:

```
DEVICESCAN  # Scan for all devices
# Para cada dispositivo
/dev/sda -a -m seu_email@example.com -M daily
/dev/sdb -a -m seu_email@example.com -M daily
```

Se você está recebendo a mensagem de erro "Unit file smartd.service does not exist", isso significa que o arquivo de serviço do `smartd` não está presente no seu sistema. Aqui estão as etapas que você pode seguir para resolver isso:

### 3. Verifique a Presença do Arquivo de Serviço

Se o `smartmontools` estiver instalado, mas o arquivo `smartd.service` não estiver presente, você pode tentar encontrar o arquivo de serviço:

```bash
find /lib/systemd/system/ -name "smartd.service"
```

Se o arquivo não existir, você precisará criá-lo manualmente.

### 4. Criar o Arquivo de Serviço `smartd.service`

Se o arquivo não for encontrado, crie um novo arquivo de serviço:

1. Crie o arquivo usando um editor de texto, como `nano`:

```bash
sudo nano /lib/systemd/system/smartd.service
```

2. Adicione o seguinte conteúdo ao arquivo:

```ini
[Unit]
Description=SMART Daemon
After=local-fs.target

[Service]
ExecStart=/usr/sbin/smartd -n
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

3. Salve e feche o arquivo (no `nano`, pressione `CTRL + X`, depois `Y` e `Enter`).

### 5. Atualizar o `systemd`

Depois de criar o arquivo de serviço, você deve recarregar o daemon do `systemd` para que ele reconheça a nova unidade:

```bash
sudo systemctl daemon-reload
```

### 6. Habilitar e Iniciar o Serviço

Agora, você pode habilitar o serviço `smartd` para iniciar no boot:

```bash
sudo systemctl enable smartd
```

E inicie o serviço:

```bash
sudo systemctl start smartd
```

### 7. Verificar o Status do Serviço

Verifique se o serviço está funcionando corretamente:

```bash
sudo systemctl status smartd
```

### 8. Verificar Logs

Se o serviço não iniciar corretamente, você pode verificar os logs do `systemd` para obter mais detalhes:

```bash
journalctl -u smartd
```

Esses passos devem ajudá-lo a configurar o serviço `smartd` para monitorar o estado SMART dos discos. Se você continuar enfrentando problemas, forneça mais detalhes sobre o que está acontecendo, e eu poderei ajudar melhor.

## Prometheus

#Prometheus/Node-Exporter 
### 1. Coletando dados com `Prometheus`

Para enviar as métricas do `smartd` para o Grafana, você pode usar o [[Prometheus]]. Para isso, utilize um exporter como o [[Prometheus#Node-Exporter|node-exporter]] que já inclui suporte para métricas de disco:

- Instale o `node_exporter`:

```bash
# Baixe e extraia o node_exporter
wget https://github.com/prometheus/node_exporter/releases/latest/download/node_exporter-<version>.linux-amd64.tar.gz
tar xvfz node_exporter-<version>.linux-amd64.tar.gz
cd node_exporter-<version>.linux-amd64

# Inicie o node_exporter
./node_exporter &
```

### 2. Configurando o `Prometheus`

Adicione o `node_exporter` ao arquivo de configuração do `Prometheus` (`prometheus.yml`):

```yaml
scrape_configs:
  - job_name: 'node'
    static_configs:
      - targets: ['localhost:9100']
```

## Grafana

#Grafana 
### 1. Instalação do Grafana

Caso ainda não tenha, instale o [[Grafana]]:

```bash
sudo apt-get install -y software-properties-common
sudo add-apt-repository -y ppa:grafana/grafana
sudo apt-get update
sudo apt-get install grafana
```

Ative e inicie o Grafana:

```bash
sudo systemctl enable grafana-server
sudo systemctl start grafana-server
```

### 2. Configurando o Grafana

1. Acesse o Grafana em `http://localhost:3000`.
2. Crie um novo datasource do tipo `Prometheus`.
3. Configure o URL do Prometheus, geralmente `http://localhost:9090`.
4. Crie um novo painel e adicione gráficos com base nas métricas do `node_exporter`, como `node_disk_*`.

### 3 Visualização de Dados

Você pode usar métricas como `node_disk_read_bytes_total`, `node_disk_written_bytes_total`, e `node_disk_io_time_seconds_total` para visualizar a saúde e desempenho do disco.
[[|Veja mais metricas de visualização de dados]]

## Referências

- https://linux.die.net/man/8/smartd
- https://prometheus.io/docs/introduction/overview/
- https://grafana.com/docs/grafana/latest/getting-started/getting-started-grafana/

Esses passos devem fornecer uma configuração básica para monitorar o estado dos discos usando SMART e exibi-los no Grafana. Se precisar de mais detalhes em alguma etapa, estou à disposição!