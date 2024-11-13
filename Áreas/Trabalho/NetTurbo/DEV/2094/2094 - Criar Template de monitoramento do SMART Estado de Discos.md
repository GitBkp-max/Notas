---
tags:
  - Trabalho/NetTurbo
  - TI/Tecnologia/Zabbix
  - TI/SMART
---
```dataview
table completed, date, startTime, endTime
from #Calendario/Trabalho/Atividades 
where contains(file.name, "2094")
sort date asc, startTime asc
```

# 2094 - Criar Template de monitoramento do SMART Estado de Discos

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
## Template ja pronto: 

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

### Explicação do template feito pelo #AI/GPT 
#### 1 - Explicação da descrição do template:

Esse template configura o monitoramento de serviços do sistema via Zabbix, utilizando SNMP e um script cron. A lógica por trás do template é a seguinte:

- **[[Cron]] job (Tarefa agendada):** A tarefa `crontab` é executada a cada minuto (`*/1 * * * *`). Ela coleta o estado de todos os serviços do sistema via `systemctl list-units` e salva o resultado em formato JSON no arquivo `/var/lib/snmp/output_files/systemctl.json`. Assim, a cada minuto, os estados dos serviços são atualizados nesse arquivo.
  
- **Diretório de saída:** O diretório `/var/lib/snmp/output_files` é criado para armazenar o arquivo JSON gerado pelo cron job. Esse arquivo contém uma listagem dos serviços, permitindo que o Zabbix tenha acesso a esses dados.
  
- **SNMPD (Agente SNMP):** Para permitir que o Zabbix acesse os dados gerados pelo cron job, você configura o SNMPD para ler o arquivo JSON. A linha `extend systemctl_state /usr/bin/cat /var/lib/snmp/output_files/systemctl.json` no arquivo `/etc/snmp/snmpd.conf` estende o SNMPD com um novo comando que executa `/usr/bin/cat` para exibir o conteúdo do arquivo JSON.

Em resumo, o cron coleta dados do estado dos serviços do sistema, salva em um arquivo, e o SNMPD permite que o Zabbix leia esse arquivo, monitorando os serviços via SNMP.

---

#### 2 - Como continuar com isso:

Você já tem a estrutura inicial do projeto. Para continuar:

1. **Configurar a tarefa cron:** Certifique-se de que a tarefa no `crontab -e` está configurada corretamente para gerar o arquivo JSON a cada minuto.

2. **Verificar permissões:** Assegure-se de que o diretório `/var/lib/snmp/output_files` e o arquivo `systemctl.json` tenham as permissões corretas para leitura pelo SNMPD e pelo script cron.

3. **Configurar o SNMPD:** Adicione a linha `extend systemctl_state /usr/bin/cat /var/lib/snmp/output_files/systemctl.json` no arquivo `/etc/snmp/snmpd.conf`. Reinicie o serviço SNMP para que as configurações entrem em vigor:
   ```bash
   sudo systemctl restart snmpd
   ```

4. **Template no Zabbix:** Configure o template no Zabbix para coletar as informações via SNMP. O Zabbix vai enviar uma requisição SNMP ao servidor, que, por sua vez, irá retornar o conteúdo do arquivo JSON gerado pelo cron job.

5. **Testar a coleta SNMP:** Teste se o SNMP está retornando o conteúdo esperado:
   ```bash
   snmpwalk -v 2c -c public localhost NET-SNMP-EXTEND-MIB::nsExtendOutputFull.\"systemctl_state\"
   ```

6. **Ajustar monitoramento no Zabbix:** Dependendo dos serviços que você deseja monitorar, ajuste o template do Zabbix para extrair as informações relevantes do JSON e configurar os gatilhos necessários.

Se precisar de mais detalhes, posso ajudar a configurar a parte do Zabbix ou adaptar o monitoramento.

---

#### 3 - Passo a passo completo para configurar o monitoramento SMART via SNMP e cron no Zabbix:

##### **1. Criação do diretório para armazenar os arquivos de saída**
O diretório onde o arquivo `systemctl.json` será gerado deve ser criado com as permissões corretas para garantir que tanto o cron quanto o SNMPD possam acessar.

```bash
sudo mkdir -p /var/lib/snmp/output_files
sudo chown -R snmp:snmp /var/lib/snmp/output_files
```

##### **2. Configuração da tarefa cron**
Você precisará criar uma entrada no cron que executa o comando para listar os serviços e salvar o resultado no arquivo JSON.

1. Abra o arquivo do cron para edição:
   ```bash
   crontab -e
   ```

2. Adicione a seguinte linha para executar a cada minuto:
   ```bash
   */1 * * * * systemctl list-units --type=service --all --no-pager --no-legend --plain --output=json > /var/lib/snmp/output_files/systemctl.json
   ```

3. Salve e feche o arquivo. Agora, a cada minuto, o cron executará o comando e gerará o arquivo JSON com o estado dos serviços.

##### **3. Configuração do SNMPD**
Agora, precisamos configurar o SNMP para que ele consiga ler e fornecer o conteúdo do arquivo `systemctl.json` para o Zabbix.

1. Abra o arquivo de configuração do SNMPD:
   ```bash
   sudo nano /etc/snmp/snmpd.conf
   ```

2. No final do arquivo, adicione a seguinte linha para estender o SNMP e permitir a leitura do arquivo JSON:
   ```bash
   extend systemctl_state /usr/bin/cat /var/lib/snmp/output_files/systemctl.json
   ```

3. Salve o arquivo e feche o editor.

4. Reinicie o serviço do SNMPD para aplicar as mudanças:
   ```bash
   sudo systemctl restart snmpd
   ```

##### **4. Teste do SNMP localmente**
Para garantir que o SNMP está configurado corretamente e consegue acessar o arquivo JSON, faça o seguinte teste usando o `snmpwalk`:

1. Execute o comando abaixo para verificar se o SNMPD está retornando o conteúdo do arquivo JSON:
   ```bash
   snmpwalk -v 2c -c public localhost NET-SNMP-EXTEND-MIB::nsExtendOutputFull.\"systemctl_state\"
   ```

2. O comando acima deve retornar o conteúdo do arquivo `/var/lib/snmp/output_files/systemctl.json`. Se o conteúdo estiver correto, o SNMP está funcionando como esperado.

##### **5. Configuração no Zabbix**
Agora vamos configurar o template no Zabbix para coletar os dados via SNMP.

1. **Acesse o Zabbix:**
   Abra o painel de administração do Zabbix e vá até a seção "Configuration" -> "Templates".

2. **Crie um novo Template:**
   - Clique em "Create template".
   - Dê um nome para o template (ex: `Template SMART Monitor`).
   - Associe o template a um grupo (ex: `Templates/Operating systems`).
   - Salve.

3. **Adicionar Item SNMP:**
   - Dentro do template criado, vá até "Items" e clique em "Create item".
   - Preencha os seguintes campos:
     - **Name:** Estado dos serviços via SNMP
     - **Type:** SNMP agent
     - **SNMP OID:** `NET-SNMP-EXTEND-MIB::nsExtendOutputFull."systemctl_state"`
     - **Key:** `service.state.snmp`
     - **Type of information:** Texto
     - **Update interval:** 1m (um minuto)
   - Salve o item.

4. **Adicionar gatilhos (opcional):**
   - Se você quiser monitorar e ser notificado sobre mudanças nos serviços, pode configurar gatilhos baseados no conteúdo do arquivo JSON. Por exemplo, verificar se algum serviço está inativo.

##### **6. Teste no Zabbix**
1. Adicione o template configurado ao host que está monitorando.
2. Espere alguns minutos para que os dados sejam coletados.
3. Verifique no Zabbix se os dados dos serviços estão sendo retornados corretamente.

##### **7. Verificar permissões (Importante)**
Certifique-se de que tanto o cron quanto o SNMPD tenham permissão de leitura e escrita no diretório e no arquivo JSON. Se necessário, execute:

```bash
sudo chown snmp:snmp /var/lib/snmp/output_files/systemctl.json
```

#### Conclusão
Agora você configurou completamente o monitoramento de serviços do sistema via SNMP e cron no Zabbix. O cron gera o arquivo JSON com o status dos serviços, o SNMPD retorna esse arquivo para o Zabbix, que então coleta e exibe essas informações.

Se precisar de ajustes, como criar mais itens ou gatilhos no Zabbix, basta expandir o template para as necessidades do seu monitoramento.