---
tags:
  - TI/Tecnologia/prometheus/Smartctl-exporter
  - TI/SMART
  - TI/Tecnologia/Grafana
---
## **Métricas do `smartmontools`** (SMART Health Monitoring)

Se você quiser monitorar especificamente a **saúde física dos discos** usando o **SMART** (Self-Monitoring, Analysis, and Reporting Technology), que é o objetivo do `smartmontools`, você precisará de métricas que fornecem informações sobre a integridade do hardware do disco, como:

### 1. **SMART Health Status**
   - **Descrição**: Status geral do disco, indicando se o disco está saudável ou próximo de falha.
   - **Utilização**: Detecta falhas iminentes. Se o status retornar como "FAILED", pode indicar que o disco precisa ser substituído.

### 2. **Power-On Hours**
   - **Descrição**: Número total de horas que o disco esteve ligado desde sua fabricação.
   - **Utilização**: Serve para monitorar a longevidade do disco. Discos com muitas horas de uso podem estar próximos do fim da vida útil.

### 3. **Reallocated Sectors Count**
   - **Descrição**: Número de setores remapeados no disco (setores defeituosos que foram substituídos por setores de reserva).
   - **Utilização**: Um aumento no número de setores remapeados pode indicar falha iminente no disco.

### 4. **Temperature**
   - **Descrição**: A temperatura atual do disco.
   - **Utilização**: Temperaturas altas podem reduzir a vida útil do disco ou até causar falhas.

### 5. **Spin Retry Count**
   - **Descrição**: Número de vezes que o disco falhou em atingir a velocidade operacional após ser ligado.
   - **Utilização**: Um aumento nesse número pode sugerir problemas mecânicos no disco.

### 6. **Uncorrectable Sector Count**
   - **Descrição**: Número de setores defeituosos que não podem ser corrigidos.
   - **Utilização**: Setores que não podem ser corrigidos indicam problemas graves que podem levar à perda de dados.

### 7. **Pending Sectors**
   - **Descrição**: Número de setores que estão esperando para serem remapeados devido a erros de leitura.
   - **Utilização**: Indica setores instáveis. Se esse número for alto, o disco pode estar próximo de falha.

### 8. **Disk Errors**
   - **Descrição**: Número de erros de leitura ou gravação.
   - **Utilização**: Um número crescente de erros pode ser um sinal precoce de falha no disco.

## Como Coletar Métricas do `smartmontools` com `Prometheus`

Para integrar esses dados do **SMART** com o [[Grafana]] via [[Prometheus]], você precisará de um **SMART Exporter** para Prometheus. Um exemplo de exporter que pode ser utilizado é o [[Prometheus#Smartctl-Exporter|smartctl_exports]], que coleta informações do `smartmontools` e as expõe para o Prometheus.

Aqui está o passo a passo detalhado para configurar o monitoramento de discos com o **`smartctl_exporter`**, integrando o `smartmontools` com Prometheus e Grafana:

### Passo 1: Instalar o `smartmontools`
Primeiro, você precisa garantir que o `smartmontools` está instalado no seu servidor para coletar os dados SMART dos discos.

1. **Instale o `smartmontools`**:

   ```bash
   sudo apt-get update
   sudo apt-get install smartmontools
   ```

2. **Verifique se o `smartd` está funcionando**:

   Ative o daemon `smartd` para monitorar continuamente o estado SMART dos discos:

   ```bash
   sudo systemctl start smartd
   sudo systemctl enable smartd
   ```

### Passo 2: Instalar o `smartctl_exporter`

O `smartctl_exporter` é um exportador que coleta as métricas SMART dos discos e as expõe para o Prometheus.

1. **Baixe e instale o `smartctl_exporter`**:

   Primeiro, vamos fazer o download do binário do `smartctl_exporter` diretamente do repositório do GitHub.

```bash
   wget https://github.com/prometheus-community/smartctl_exporter/releases/download/v0.12.0/smartctl_exporter-0.12.0.linux-amd64.tar.gz -O smartctl_exporter
```


> [!caution] Atenção
> Busque sempre a versão valida do aquivo caso o link não estiver funcionando
> ### Passo 1: Encontrar a versão mais recente do `smartctl_exporter`
> 1. Acesse o repositório oficial no GitHub:   
> [https://github.com/prometheus-community/smartctl_exporter/releases](https://github.com/analogj/smartctl_exporter/releases)
>2. Localize a versão mais recente do `smartctl_exporter` compatível com o seu sistema.
>3. Substitua o link do `wget` pelo novo link encontrado.
>Exemplo: Se a versão mais recente for `v0.5.0`, o comando atualizado seria:
>```bash
>wget https://github.com/prometheus-community/smartctl_exporter/releases/download/v0.5.0/smartctl_exporter_linux_amd64 -O smartctl_exporter
>```
>### Passo 2: Continuar com o processo de instalação
>Após baixar o binário correto, continue com os passos já descritos anteriormente para dar permissão ao arquivo e configurar o `smartctl_exporter`.

 3. Extrair o conteúdo corretamente

Agora, extraia o arquivo `.tar.gz`:

```bash
tar -xvzf smartctl_exporter-0.12.0.linux-amd64.tar.gz
```

Isso deve gerar um diretório com o nome `smartctl_exporter-0.12.0.linux-amd64`.

4. **Verificar o binário extraído**

Navegue até o diretório extraído e verifique o binário:

```bash
cd smartctl_exporter-0.12.0.linux-amd64
file ./smartctl_exporter
```

Isso deve confirmar que o binário é um executável compatível com **x86_64**. O resultado deve ser algo como:

```bash
smartctl_exporter: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked, ...
```

5. **Mover o binário e torná-lo executável**

```bash
chmod +x smartctl_exporter
sudo mv smartctl_exporter /usr/local/bin/
```

### Passo 3: Executar o `smartctl_exporter`

1. **Executar o `smartctl_exporter`**:

Agora você pode iniciar o `smartctl_exporter`. Ele abrirá um servidor HTTP que o Prometheus pode acessar para coletar as métricas SMART.

```bash
sudo /usr/local/bin/smartctl_exporter
```

   O exportador estará ouvindo na porta `9633` por padrão.

2. **Executar o `smartctl_exporter` como um serviço (opcional)**:

   Para que o `smartctl_exporter` seja executado automaticamente no boot, crie um arquivo de serviço `systemd`.

   1. **Crie o arquivo de serviço**:

```bash
sudo nano /etc/systemd/system/smartctl_exporter.service
```

   2. **Adicione o seguinte conteúdo**:

```ini
[Unit]
Description=smartctl exporter
After=network.target

[Service]
ExecStart=/usr/local/bin/smartctl_exporter
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

   3. **Salve e feche o arquivo** (`Ctrl+X`, depois `Y` e `Enter`).

   4. **Ative o serviço**:

```bash
sudo systemctl daemon-reload
sudo systemctl enable smartctl_exporter
sudo systemctl start smartctl_exporter
```

### Passo 4: Configurar o Prometheus

Agora, você precisa configurar o **Prometheus** para coletar as métricas do `smartctl_exporter`.

1. **Abra o arquivo de configuração do Prometheus** (`prometheus.yml`):

```bash
sudo nano /opt/prometheus/prometheus.yml
```

2. **Adicione o seguinte bloco à configuração**:

   ```yaml
   scrape_configs:
     - job_name: 'smartctl'
       static_configs:
         - targets: ['localhost:9633']
   ```

   Isso diz ao Prometheus para monitorar o `smartctl_exporter` na porta 9633 do seu servidor.

3. **Salve e feche o arquivo**.

4. **Reinicie o Prometheus para aplicar as mudanças**:

```bash
sudo systemctl restart prometheus
```

### Passo 5: Configurar o Grafana

Agora que as métricas do SMART estão sendo coletadas pelo Prometheus, você pode criar gráficos no Grafana.

1. **Abra o Grafana e adicione o Prometheus como fonte de dados**:
   - Vá para **Configuration** > **Data Sources** > **Add data source**.
   - Escolha **Prometheus** e configure a URL para `http://localhost:9090`.

2. **Crie um novo Dashboard**:
   - Vá para **Dashboards** > **New Dashboard** > **Add Query**.
   - Escolha a fonte de dados **Prometheus**.

3. **Exemplo de Query para o Grafana**:
   - Para verificar o status geral dos discos, use a seguinte query:

```promql
smartctl_device_smart_healthy
```

     Isso retornará `1` se o disco estiver saudável, ou `0` se houver problemas.

4. **Outras queries úteis**:
   - **Horas de uso do disco**:

```promql
smartctl_device_power_on_hours
```

   - **Temperatura do disco**:

```promql
smartctl_device_temperature_celsius
```

   - **Contagem de setores remapeados**:

     ```promql
     smartctl_device_reallocated_sector_count
     ```

5. **Personalize os gráficos** de acordo com suas necessidades, exibindo a saúde dos discos, temperatura e outros indicadores relevantes.

### Passo 6: Verificar o Funcionamento

Agora que tudo está configurado:

1. **Verifique se o `smartctl_exporter` está rodando corretamente**:

   Acesse no navegador:

   ```http://<IP_DO_SEU_SERVIDOR>:9633/metrics```

   Deve listar todas as métricas SMART dos seus discos.

2. **Verifique as métricas no Prometheus**:

   Acesse a interface web do Prometheus (`http://localhost:9090/graph`) e procure por métricas como `smartctl_device_smart_healthy`.

3. **Monitore no Grafana**:

   Visualize seus gráficos no dashboard criado.

### Referências:
- `smartmontools`: https://www.smartmontools.org/
- `smartctl_exporter`: https://github.com/analogj/smartctl_exporter
- Documentação do Prometheus: https://prometheus.io/docs

Isso deve cobrir suas necessidades para monitorar tanto o desempenho quanto a saúde dos discos. Se precisar de mais detalhes ou ajuda com a configuração, posso orientar.