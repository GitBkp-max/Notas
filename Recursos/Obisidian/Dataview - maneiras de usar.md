---
tags:
  - Obsidian/Plugin
---
O plugin **Dataview** do #Obsidian permite criar visualizações dinâmicas de informações a partir de notas com base em consultas semelhantes ao SQL. É ideal para organizar e filtrar dados armazenados em tags, cabeçalhos, propriedades frontmatter e até mesmo conteúdo de tabelas em suas notas. Aqui estão alguns comandos básicos para iniciar com o Dataview:

### 1. **Configurar a estrutura da nota com Frontmatter**

Adicione um bloco `YAML frontmatter` no início de cada nota para definir metadados. Isso é essencial para que o Dataview identifique os campos e filtre as notas com base neles.

```yaml
---
title: "Título da Nota"
date: "2024-10-25"
tags: [projeto, desenvolvimento]
status: "Em andamento"
---
```
### 2. **Criar uma Consulta Básica**

O comando básico `table` exibe os dados em uma tabela. Exemplo para listar notas com a tag `projeto`:

```
table title, date, status
from "caminho/da/pasta"
where contains(tags, "projeto")
sort date desc
```

### 3. **Consultar usando `list` e `task`**

- **Lista**: Retorna itens como uma lista, útil para resumos rápidos.

```
list from "caminho/da/pasta"
where status = "Concluído
```

- **Tarefas**: Exibe caixas de tarefas em um projeto ou lista de notas.

```
task from "caminho/da/pasta"
where contains(tags, "tarefa") and status != "Concluído"
```

### 4. **Funções e Cálculos**

É possível fazer cálculos básicos, como contar quantas notas possuem um determinado status:

```
table count(status) as "Total"
from "caminho/da/pasta"
where status = "Em andamento"
```

Esses exemplos iniciais cobrem o uso básico do Dataview no Obsidian