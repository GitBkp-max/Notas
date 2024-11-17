---
tags:
  - TI/Tecnologia/Git-GitHub
---


---

## O problema dos commits não semânticos

Você já se deparou com mensagens de commit como:
- "Subindo alterações"
- "Ajustes..."
- "Correção de bugs"?

Essas mensagens vagas podem prejudicar a colaboração em equipe e dificultar o rastreamento de alterações no projeto. O que parece ser uma simples mudança pode tornar o processo de revisão, depuração e manutenção muito mais complexo.


---
## O que são commits semânticos?

Commits semânticos seguem um padrão claro e descritivo, que facilita a compreensão exata das modificações feitas no código. Eles combinam uma estrutura consistente com uma mensagem significativa, permitindo que o histórico de mudanças seja organizado, compreensível e acessível a todos na equipe.

Os commits semânticos ajudam a identificar rapidamente o propósito de cada modificação e fornecem contexto para revisões futuras, como ao investigar bugs ou ao realizar auditorias de código. Isso é crucial para manter um código sustentável e colaborativo em projetos de longo prazo.

---
## Tipos de commits semânticos e quando utilizá-los

---

### ✨ feat: Adição de nova funcionalidade.
Use `feat` para introduzir algo novo no código, como uma nova funcionalidade ou um recurso que não existia antes. Esse tipo de commit indica que há um incremento significativo na aplicação.

**Exemplo**: 
- `✨ feat(auth): adicionar autenticação com OAuth`

Quando utilizar: sempre que estiver adicionando algo que expande a funcionalidade do sistema, como uma nova tela, endpoint ou componente.

---

### 🐛 fix: Correção de bugs.
Use `fix` para commits que corrigem falhas no código. Estes são extremamente importantes, pois indicam que algo que estava quebrado foi reparado, permitindo um rápido rastreamento de problemas.

**Exemplo**: 
- `🐛 fix(login): corrigir erro na validação de senha`

Quando utilizar: sempre que resolver um problema existente, seja ele um bug reportado ou algo que você encontrou durante o desenvolvimento.

---

### 📚 docs: Alteração na documentação.
Use `docs` para commits que fazem alterações exclusivamente em arquivos de documentação, como README, guias de contribuição ou documentações de APIs.

**Exemplo**: 
- `📚 docs(readme): atualizar instruções de instalação`

Quando utilizar: qualquer alteração que não envolva código funcional, mas sim documentação, tutoriais ou instruções.

---

### ♻️ refactor: Refatoração de código.
Use `refactor` para mudanças que alteram o código sem modificar o comportamento externo. Este tipo de commit é utilizado quando o código é reestruturado para melhorar sua legibilidade, organização ou eficiência, mas sem adicionar novas funcionalidades ou corrigir bugs.

**Exemplo**: 
- `♻️ refactor(api): simplificar lógica de autenticação`

Quando utilizar: sempre que melhorar o código sem alterar sua funcionalidade, como remoção de duplicidades, simplificação de algoritmos ou reestruturação de classes.

---

### 🛠️ chore: Tarefas de manutenção.
Use `chore` para mudanças que não afetam diretamente o código ou comportamento do software, mas que são necessárias para manter o projeto organizado. Isso pode incluir atualizações de dependências, mudanças na configuração de builds ou ajustes nos scripts de automação.

**Exemplo**: 
- `🛠️ chore(deps): atualizar dependências do projeto`

Quando utilizar: em ajustes internos que não afetam a base de código principal, mas que são necessários para manter o projeto funcionando corretamente.

---

### ⚡ perf: Melhorias de desempenho.
Use `perf` para commits que melhoram a performance do código. Este tipo de commit é ideal quando você otimiza o uso de recursos como CPU, memória ou processamento.

**Exemplo**: 
- `⚡ perf(query): otimizar consulta no banco de dados`

Quando utilizar: ao refatorar trechos de código para melhorar o desempenho, sem alterar o comportamento funcional.

---

### 🚨 test: Adição ou alteração em testes.
Use `test` para commits relacionados a testes. Seja a criação de novos testes, ajustes em testes existentes ou melhorias no ambiente de testes.

**Exemplo**: 
- `🚨 test(api): adicionar testes para validação de login`

Quando utilizar: sempre que adicionar ou modificar testes automatizados para garantir a qualidade do código.

---

### 🚧 wip: Trabalho em progresso.
Use `wip` (work in progress) para commits intermediários que ainda não estão prontos para serem concluídos, mas que precisam ser registrados para continuar o trabalho posteriormente. É ideal para projetos colaborativos onde múltiplos desenvolvedores podem estar trabalhando na mesma parte do sistema.

**Exemplo**: 
- `🚧 wip(ui): início da implementação do layout de dashboard`

Quando utilizar: quando você ainda está desenvolvendo algo, mas precisa salvar o progresso atual. Isso é útil especialmente em colaborações onde outros desenvolvedores podem revisar seu trabalho.

---
Para manter a consistência e a clareza nos seus commits durante um merge, você pode usar o seguinte modelo, baseado nos princípios dos commits semânticos:

---

### 🔀 merge: Mensagem clara e objetiva para um commit de merge

**Formato**:  
`🔀 merge(escopo): breve descrição do merge`

**Exemplos**:  
- `🔀 merge(branch-hotfix): integrar correções críticas na main`  
- `🔀 merge(feature-auth): mesclar nova funcionalidade de autenticação`  
- `🔀 merge(dev → main): integrar alterações finalizadas no ambiente de desenvolvimento`  

---

#### Estrutura detalhada para commits de merge:

1. **Tipo**: Use `merge` para indicar que este commit é o resultado da mesclagem de branches.
2. **Escopo (opcional)**: Descreva o branch ou a funcionalidade principal envolvida no merge (por exemplo, `branch`, `feature`, `hotfix`).
3. **Descrição**: Adicione uma descrição breve e clara sobre o objetivo do merge. Você pode mencionar os branches envolvidos e o motivo do merge.

---

#### Modelo para commits de conflitos resolvidos

Caso haja resolução de conflitos, inclua uma nota no commit para facilitar o rastreamento:

**Formato**:  
`🔀 merge(escopo): resolver conflitos durante integração`

**Exemplos**:  
- `🔀 merge(feature-ui): resolver conflitos entre feature-ui e main`  
- `🔀 merge(dev → release): corrigir conflitos de versão antes de mesclar alterações`

**Dica**: Se possível, mencione arquivos ou módulos principais que tiveram conflitos resolvidos, especialmente se isso for relevante para revisões futuras. Por exemplo:  
- `🔀 merge(feature-login): resolver conflitos no módulo auth.js`  

---

#### Boas práticas para mensagens de merge
1. **Seja objetivo**: Deixe claro qual é o propósito do merge e quais branches estão envolvidos.
2. **Contextualize**: Se o merge inclui mudanças significativas ou resolução de conflitos, destaque essas informações.
3. **Mantenha o padrão**: Mesmo em merges, siga o formato semântico para manter o histórico de commits claro e legível.

Usando esses modelos, você garantirá que os merges fiquem bem documentados no histórico do projeto! 🚀
## Como estruturar um commit semântico

A estrutura de um commit semântico é fundamental para garantir sua clareza. O formato básico é:

**Formato**:  
`<tipo>(escopo opcional): breve descrição`

**Exemplo**:  
`✨ feat(api): adicionar autenticação com OAuth`

- O **tipo** define a natureza do commit.
- O **escopo** (opcional) especifica a parte do projeto afetada, como um módulo, componente ou serviço.
- A **breve descrição** dá um resumo conciso da mudança feita.

Esse formato garante que seu projeto siga um padrão lógico e consistente, facilitando a comunicação entre os desenvolvedores e melhorando o rastreamento de mudanças.

---
## Vantagens de utilizar commits semânticos

Adotar commits semânticos traz uma série de benefícios para o desenvolvimento de software, como:
- **Facilidade de leitura**: permite que outros desenvolvedores entendam rapidamente o que foi feito em cada alteração.
- **Rastreamento de bugs**: facilita o diagnóstico de problemas ao examinar o histórico de commits.
- **Automação**: muitas ferramentas de CI/CD (integração contínua/entrega contínua) podem ser configuradas para realizar ações automáticas com base nos tipos de commits, como gerar changelogs automaticamente.

Um bom commit é a chave para um projeto bem organizado e de fácil manutenção!

---

