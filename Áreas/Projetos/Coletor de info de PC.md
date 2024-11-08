### Informação sobre o Script `.bat` Criado

#### Descrição Geral
Este script `.bat` foi criado para realizar uma série de tarefas administrativas no sistema operacional Windows. Ele executa as seguintes ações principais:

1. **Limpeza das pastas temporárias do sistema e do usuário.**
2. **Coleta de informações de hardware e sistema.**
3. **Geração de um relatório de exclusão e um resumo das informações coletadas.**

#### Funcionalidades do Script

1. **Limpeza das Pastas Temporárias:**
   - O script limpa a pasta `%temp%` do usuário atual, removendo todos os arquivos e diretórios que não estão em uso.
   - Também limpa a pasta `C:\Windows\Temp`, removendo todos os arquivos e diretórios não utilizados.
   - Relata quantos arquivos e diretórios foram excluídos com sucesso e quantas falhas ocorreram (arquivos em uso).

2. **Coleta de Informações do Sistema:**
   - **Nome da Máquina:** Obtém e utiliza o nome da máquina para nomear o arquivo de informações.
   - **Serial da Máquina:** Utiliza o comando `wmic bios get serialnumber` para coletar o número de série.
   - **Informações do Processador:** Coleta o nome do processador usando `wmic cpu get name`.
   - **Modelo da Máquina:** Coleta o modelo da máquina usando `wmic computersystem get model`.
   - **Marca da Máquina:** Coleta a marca (fabricante) da máquina usando `wmic computersystem get manufacturer`.
   - **Quantidade de Memória RAM:** Calcula a quantidade total de memória RAM em gigabytes usando `wmic os get TotalVisibleMemorySize`.
   - **Tipo de Memória RAM (DDR):** Obtém o tipo de memória RAM (por exemplo, DDR3, DDR4) usando `wmic memorychip get memorytype`.

3. **Geração do Relatório:**
   - Cria um arquivo de texto no mesmo diretório onde o script está localizado. O nome do arquivo segue o formato `{nome_da_máquina}_info.txt`.
   - Adiciona a data e a hora da coleta de informações ao arquivo.
   - Gera um relatório detalhado de quantos arquivos e diretórios foram excluídos com sucesso ou falharam na exclusão em ambas as pastas (`%temp%` e `C:\Windows\Temp`).

#### Como Utilizar

1. **Executar o Script:**
   - Faça o download ou crie o arquivo `.bat` com o conteúdo fornecido.
   - Clique com o botão direito do mouse no arquivo `.bat` e selecione "Executar como administrador" para garantir que todas as permissões necessárias sejam concedidas.

2. **Relatório Gerado:**
   - O script gera um arquivo de texto no mesmo diretório onde o script foi executado.
   - O nome do arquivo será `{nome_da_máquina}_info.txt`, onde `{nome_da_máquina}` é o nome do host da máquina.
   - O arquivo conterá todas as informações coletadas sobre o sistema, incluindo detalhes sobre a limpeza das pastas temporárias.

#### Exemplo de Saída do Relatório

```txt
SerialNumber=XXXX-XXXX-XXXX-XXXX
Name=Intel(R) Core(TM) i7-XXXX CPU @ X.XXGHz
Model=Modelo da Máquina
Manufacturer=Fabricante da Máquina
Quantidade de memória RAM: 16 GB
Tipo de DDR: 24

Data e hora da coleta: 
24/05/2024
08:16

Relatório de exclusão de arquivos e diretórios:
Arquivos e diretórios excluídos com sucesso na pasta %temp%: 15
Falhas ao excluir arquivos e diretórios na pasta %temp%: 2
Arquivos e diretórios excluídos com sucesso na pasta C:\Windows\Temp: 10
Falhas ao excluir arquivos e diretórios na pasta C:\Windows\Temp: 3

Informações coletadas com sucesso. Verifique o arquivo NB506919_info.txt no diretório atual.
```

#### Benefícios

- **Automação:** Automatiza a tarefa de limpeza de arquivos temporários, economizando tempo.
- **Informação Detalhada:** Fornece um relatório detalhado das informações do sistema e do hardware, útil para inventário e manutenção.
- **Manutenção Preventiva:** Ajuda na manutenção preventiva ao limpar arquivos temporários que podem ocupar espaço desnecessário no disco.

Este script é uma ferramenta útil para administradores de sistemas e usuários avançados que desejam manter seu sistema limpo e ter uma visão clara das especificações do hardware e do sistema.

# Codigo


```powershell
@echo off
:: Limpar a pasta temp do usuário atual
echo Limpando a pasta %temp%...
del /f /q "%temp%\*"
for /d %%x in ("%temp%\*") do @rd /s /q "%%x"

:: Limpar a pasta temp do Windows
echo Limpando a pasta C:\Windows\Temp...
del /f /q "C:\Windows\Temp\*"
for /d %%x in ("C:\Windows\Temp\*") do @rd /s /q "%%x"

:: Obter o nome da máquina
for /f "skip=1 tokens=*" %%a in ('wmic computersystem get name') do if not defined COMPUTER_NAME set COMPUTER_NAME=%%a
set COMPUTER_NAME=%COMPUTER_NAME: =%
echo Nome da máquina: %COMPUTER_NAME%

:: Criar o arquivo de informações no diretório atual com o nome da máquina
set INFO_FILE=%~dp0%COMPUTER_NAME%_info.txt
echo Caminho do arquivo de informações: %INFO_FILE%
if exist %INFO_FILE% del /f /q %INFO_FILE%

:: Obter o Serial da Máquina
echo Obtendo o Serial da Máquina...
wmic bios get serialnumber /value | find "=" >> %INFO_FILE%

:: Obter o MAC do WiFi
echo Obtendo o MAC do WiFi...
ipconfig /all > %temp%\ipconfig.txt
set "MAC="
setlocal enabledelayedexpansion
for /f "tokens=*" %%A in (%temp%\ipconfig.txt) do (
    set "line=%%A"
    echo !line! | findstr /i "Adaptador de Rede sem Fio Wi-Fi" >nul && set "capture=yes"
    if defined capture (
        echo !line! | findstr /i "Endereço Físico" >nul && (
            for /f "tokens=2 delims=:" %%B in ("!line!") do set "MAC=%%B"
            set "capture="
        )
    )
)
endlocal & set "MAC=%MAC:~1%"
echo MAC do WiFi: %MAC% >> %INFO_FILE%
del /f /q %temp%\ipconfig.txt

:: Obter informações do Processador
echo Obtendo informações do Processador...
wmic cpu get name /value | find "=" >> %INFO_FILE%

:: Obter o Modelo da Máquina
echo Obtendo o Modelo da Máquina...
wmic computersystem get model /value | find "=" >> %INFO_FILE%

:: Obter a Marca da Máquina
echo Obtendo a Marca da Máquina...
wmic computersystem get manufacturer /value | find "=" >> %INFO_FILE%

:: Obter a quantidade de memória RAM
echo Obtendo quantidade de memória RAM...
for /f "tokens=1" %%m in ('wmic os get TotalVisibleMemorySize /value ^| find "="') do (
    set /a "RAM_GB=%%m / 1000 / 1000"
)
echo Quantidade de memória RAM: %RAM_GB% GB >> %INFO_FILE%

echo Informações coletadas com sucesso. Verifique o arquivo %INFO_FILE% no diretório atual.

pause

```