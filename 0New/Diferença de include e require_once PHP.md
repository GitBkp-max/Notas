A principal diferença entre as funções `include` e `require_once` no PHP está relacionada à forma como elas tratam erros e ao comportamento em relação ao carregamento repetido de arquivos.

Aqui está uma análise detalhada:

---

### **1. `include`**
- **O que faz:** 
  - Inclui e avalia o arquivo especificado.  
  - Se o arquivo não for encontrado ou houver um erro, o PHP lança um *warning* (aviso), mas o script continuará executando.
  
- **Uso típico:**  
  - Utilizado quando a ausência do arquivo não é crítica para a execução do script.  

- **Exemplo:**
  ```php
  include 'arquivo.php';
  echo "Este código será executado mesmo se 'arquivo.php' não for encontrado.";
  ```

---

### **2. `require_once`**
- **O que faz:**
  - Similar ao `require`, inclui e avalia o arquivo especificado.
  - Se o arquivo não for encontrado, o PHP lança um *fatal error* (erro fatal) e interrompe a execução do script.
  - O `require_once` tem a garantia de incluir o arquivo apenas uma vez, mesmo que seja chamado várias vezes no código.

- **Uso típico:**  
  - Utilizado quando o arquivo é essencial para a execução do script e você deseja evitar a inclusão repetida do mesmo arquivo.

- **Exemplo:**
  ```php
  require_once 'arquivo.php';
  echo "Este código só será executado se 'arquivo.php' for encontrado e incluído.";
  ```

---

### **Comparação entre `include` e `require_once`**
| **Aspecto**                     | **include**                               | **require_once**                            |
| ------------------------------- | ----------------------------------------- | ------------------------------------------- |
| **Erro se arquivo não existir** | Gera um *warning* e continua o script     | Gera um *fatal error* e interrompe o script |
| **Inclusão múltipla**           | Pode incluir o mesmo arquivo várias vezes | Inclui o arquivo apenas uma vez             |
| **Uso recomendado**             | Arquivos opcionais ou não críticos        | Arquivos essenciais e únicos                |

---

### **Casos de Uso**
1. **Quando usar `include`:**
   - Arquivos de layout ou de mensagens opcionais.
   - Scripts que não dependem exclusivamente do arquivo para funcionar.

   ```php
   include 'header.php';
   include 'footer.php';
   ```

2. **Quando usar `require_once`:**
   - Arquivos críticos, como bibliotecas, conexões de banco de dados ou arquivos de configuração.
   - Garantir que um arquivo não seja incluído mais de uma vez, evitando erros como redefinição de funções ou classes.

   ```php
   require_once 'config.php';
   require_once 'database.php';
   ```

---

### **Nota sobre `include_once` e `require_once`**
- Ambas garantem que o arquivo seja incluído **apenas uma vez**.
- A diferença entre `include_once` e `require_once` segue a mesma lógica básica de `include` e `require`:
  - `include_once` gera um *warning* se o arquivo não existir.
  - `require_once` gera um *fatal error* se o arquivo não existir.

Se precisar de mais exemplos ou dúvidas sobre o tema, é só avisar, Marcos!