---
tags:
  - Antivirus
  - SO/Linux
---
Para usar o ClamAV no Linux, você precisará instalar o software e aprender os comandos básicos para escanear e gerenciar o antivírus. Aqui está um guia básico para começar:

## Instalação do ClamAV:

Se você estiver usando uma distribuição Linux baseada em Debian, como o Ubuntu, você pode instalar o ClamAV usando o gerenciador de pacotes `apt`. Abra o terminal e digite o seguinte comando:

```bash
sudo apt update
sudo apt install clamav
```

Se estiver usando outra distribuição Linux, consulte a documentação ou os repositórios de pacotes específicos para sua distribuição.

## Atualização das definições de vírus:

Após a instalação, você deve atualizar as definições de vírus para garantir que o ClamAV tenha conhecimento das últimas ameaças. Execute o seguinte comando no terminal:

```bash
sudo freshclam
```

## **Configuração dos arquivos de configuração**:

O ClamAV vem com vários arquivos de configuração que você pode ajustar de acordo com suas necessidades. Os principais arquivos de configuração são `clamd.conf` e `freshclam.conf`. Esses arquivos estão localizados em `/etc/clamav/`.

## **Configuração do serviço ClamAV**:

Se você deseja que o ClamAV funcione como um serviço em segundo plano para proteger seu sistema em tempo real, você deve garantir que o serviço `clamav-daemon` esteja configurado corretamente para iniciar automaticamente no boot. Você pode verificar o status do serviço e iniciar, parar ou reiniciar conforme necessário usando os seguintes comandos:

```bash
sudo systemctl status clamav-daemon
sudo systemctl start clamav-daemon
sudo systemctl stop clamav-daemon
sudo systemctl restart clamav-daemon
```

## Escaneamento de arquivos e diretórios:

Para escanear um arquivo ou diretório específico, use o comando `clamscan`. Por exemplo, para escanear um diretório chamado "Documents", você pode usar o seguinte comando:

```bash
clamscan -r /home/seu_usuario/Documents
```

Isso escaneará o diretório `Documents` e seus subdiretórios recursivamente.
## Opções úteis do ClamAV:

- `r` : Opção para escanear recursivamente subdiretórios.
- `i` : Mostra apenas os arquivos infectados.
- `v` : Modo de exibição detalhado.
- `-remove` : Remove arquivos infectados (use com cautela).
## Programação de escaneamentos regulares:
[[Configurar escaneamentos regulares com o ClamAV]]
Você pode agendar escaneamentos regulares com o cron do Linux. Edite o arquivo cron com o comando `crontab -e` e adicione uma linha para especificar quando e como você deseja executar o escaneamento.

Por exemplo, para agendar um escaneamento todos os domingos às 3 da manhã, você poderia adicionar esta linha:

```bash
0 3 * * 0 clamscan -r /home/seu_usuario
```

Este é um exemplo básico de como usar o ClamAV no Linux. Certifique-se de ler a documentação oficial e explorar mais opções e recursos para se ajustar às suas necessidades específicas.