---
tags:
  - SO/Linux
---
Configurar um firewall no seu servidor Linux é uma prática essencial para garantir a segurança e proteger o sistema contra acessos indesejados. O `ufw` (Uncomplicated Firewall) é uma ferramenta que simplifica essa tarefa, especialmente em distribuições baseadas em Debian, como Ubuntu.

---
## Passos para Configurar o `ufw` no seu Linux Server (Ubuntu 22.04 LTS)

---
### 1. **Verifique se o `ufw` está instalado**

Na maioria das distribuições baseadas em Debian, o `ufw` já vem pré-instalado. Para verificar se está instalado, execute:

```bash
sudo ufw status
```

Se o `ufw` não estiver instalado, você pode instalá-lo com o seguinte comando:

```bash
sudo apt update
sudo apt install ufw
```

---
### 2. **Configurar as Regras do `ufw`**

Antes de habilitar o `ufw`, é recomendável configurar as regras para garantir que você não perca o acesso ao servidor. Abaixo estão alguns exemplos de comandos para configurar as regras básicas:

- **Permitir SSH (porta 22):** Essencial para manter o acesso remoto ao servidor.
    ```bash
    sudo ufw allow ssh
    ```
    
- **Permitir Acesso HTTP (porta 80):** Se o servidor está rodando um site ou aplicação web.
    
    ```bash
    sudo ufw allow http
    ```
    
- **Permitir Acesso HTTPS (porta 443):** Se o servidor está usando SSL/TLS.
    
    ```bash
    sudo ufw allow https
    ```
    
- **Permitir Outras Portas:** Para permitir outras portas específicas, você pode especificar a porta diretamente, por exemplo:
    
    ```bash
    sudo ufw allow 8080
    ```
    
- **Negar Acesso a Portas Específicas:** Para bloquear uma porta específica, use:
    
    ```bash
    sudo ufw deny 12345
    ```
    

---
### 3. **Ativar o `ufw`**

Após configurar as regras desejadas, você pode ativar o firewall com o seguinte comando:

```bash
sudo ufw enable
```

Você será solicitado a confirmar a ativação do `ufw`. Após isso, o firewall estará ativo e aplicando as regras configuradas.

---
### 4. **Verificar o Status do `ufw`**

Para garantir que o `ufw` está ativo e as regras estão corretas, use:

```bash
sudo ufw status verbose
```

Isso mostrará todas as regras que estão sendo aplicadas e o status do firewall.

---
### 5. **Configurações Adicionais**

- **Permitir ou Bloquear IPs Específicos:**
    
    Para permitir o acesso de um IP específico:
    
    ```bash
    sudo ufw allow from 192.168.1.100
    ```
    
    Para bloquear um IP específico:
    
    ```bash
    sudo ufw deny from 192.168.1.100
    ```
    
- **Desabilitar o `ufw`:** Se em algum momento você precisar desativar o firewall, use:
    
    ```bash
    sudo ufw disable
    ```
    

---
### 6. **Logs do `ufw`**

Para monitorar as atividades e possíveis tentativas de acesso bloqueadas, você pode habilitar o registro de logs:

```bash
sudo ufw logging on
```

Os logs serão armazenados em `/var/log/ufw.log`.

---
### Conclusão

Configurar o `ufw` no seu servidor Linux é uma forma simples e eficaz de proteger o sistema. Com a configuração correta das regras de firewall, você pode garantir que apenas o tráfego autorizado tenha acesso ao seu servidor, minimizando os riscos de segurança.