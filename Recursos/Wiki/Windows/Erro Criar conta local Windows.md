---
tags:
  - SO/Windows
---

> [!NOTE] Mensagem
> Algo deu errado ao criar uma conta local de usuário no Windows 10 erro - Tente novamente ou selecione Cancelar para configurar seu dispositivo mais tarde

Pode ocorrer que as entradas do Registro para NetBT foram corrompidos, incorreto ou faltando. Siga estes passos para verificar:

**Importante:** esta seção, método, ou tarefa contém etapas que informam sobre como modificar o registro. No entanto, sérios problemas poderão ocorrer se você modificar o registro incorretamente. Portanto, certifique-se que você execute estas etapas cuidadosamente. Para proteção adicional, fazer backup do registro antes de modificá-lo. Você poderá restaurar o registro se ocorrer um problema. Para mais informações sobre como fazer backup e restaurar o registro, clique no artigo abaixo:

Para verificar o registro siga os passos abaixo:

1. Na Área de trabalho, pressione simultaneamente nas teclas **Windows + R** para abrir o **Executar;**
2. No Executar digite **REGEDIT** para abrir o Editor de Registro;
3. Localize a seguinte chave de registro:

- **HKEY_LOCAL_MACHINE \ SYSTEM \ CurrentControlSet \ Services \ NetBT \ Parameters**
- Verifique se o valor "**TransportBindName**" existe. Para cria-lo clique com o botão direito do mouse sobre um espaço vazio e clique em novo e depois em Valor da cadeia de caracteres. Se ele não existir, terá que criá-lo. Se estiver incorreto, altere os dados. Os valores devem ser como este:
- **TransportBindName - Tipo: REGSZ - Valor: \ Device \**
- Sob a mesma chave de registro, se você encontrar o valor "**SMBDeviceEnabled**" existe e é **0**, que significa Direct Hosting está **desativado**, (novamente, se ele não aparecer lá, criar como o acima, apenas certifique-se de seu tipo é **REG_DWORD** ). Você pode alterar o seu valor para **1** para liga-lo a menos que existam requisitos que esta deve ser desativado.

**SMBDeviceEnabled**

**Tipo: REG_DWORD**

**Valor: 1**

Fechar editor do Registro e verificar se você pode logar em sua conta.

Caso não resolva, crie uma nova conta de usuário e verifique o comportamento.

Para criar uma nova conta siga os passos abaixo:

1. Pressione as teclas simultaneamente Windows + X;
2. Nas opções que se apresentam clique em Prompt de Comando (Admin);
3. No Prompt de Comando digite as seguintes linhas:
    - net user teste /add (enter)
    - net localgroup administradores teste /add (enter)

Entre com a conta Teste e adicione sua conta de e-mail.