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

### Estrutura detalhada para commits de merge:

1. **Tipo**: Use `merge` para indicar que este commit é o resultado da mesclagem de branches.
2. **Escopo (opcional)**: Descreva o branch ou a funcionalidade principal envolvida no merge (por exemplo, `branch`, `feature`, `hotfix`).
3. **Descrição**: Adicione uma descrição breve e clara sobre o objetivo do merge. Você pode mencionar os branches envolvidos e o motivo do merge.

---

### Modelo para commits de conflitos resolvidos

Caso haja resolução de conflitos, inclua uma nota no commit para facilitar o rastreamento:

**Formato**:  
`🔀 merge(escopo): resolver conflitos durante integração`

**Exemplos**:  
- `🔀 merge(feature-ui): resolver conflitos entre feature-ui e main`  
- `🔀 merge(dev → release): corrigir conflitos de versão antes de mesclar alterações`

**Dica**: Se possível, mencione arquivos ou módulos principais que tiveram conflitos resolvidos, especialmente se isso for relevante para revisões futuras. Por exemplo:  
- `🔀 merge(feature-login): resolver conflitos no módulo auth.js`  

---

### Boas práticas para mensagens de merge
1. **Seja objetivo**: Deixe claro qual é o propósito do merge e quais branches estão envolvidos.
2. **Contextualize**: Se o merge inclui mudanças significativas ou resolução de conflitos, destaque essas informações.
3. **Mantenha o padrão**: Mesmo em merges, siga o formato semântico para manter o histórico de commits claro e legível.

Usando esses modelos, você garantirá que os merges fiquem bem documentados no histórico do projeto! 🚀