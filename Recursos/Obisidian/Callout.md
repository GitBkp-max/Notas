---
tags:
  - Obsidian
---
## Chamadas

Use frases de destaque para incluir conteúdo adicional sem interromper o fluxo de suas anotações.

Para criar um texto explicativo, adicione `[!info]`à primeira linha de um blockquote, onde `info`está o _identificador do tipo_ . O identificador de tipo determina a aparência e o comportamento da chamada. Para ver todos os tipos disponíveis, consulte [Tipos suportados](https://help.obsidian.md/Editing+and+formatting/Callouts#Supported%20types) .

```markdown
> [!info]
> Here's a callout block.
> It supports **Markdown**, [[Internal link|Wikilinks]], and [[Embed files|embeds]]!
> ![[Engelbart.jpg]]
```

> [!info]
> Here's a callout block.
> It supports **Markdown**, [[Internal link|Wikilinks]], and [[Embed files|embeds]]!
> ![Engelbart.jpg](https://publish-01.obsidian.md/access/f786db9fac45774fa4f0d8112e232d67/Attachments/Engelbart.jpg)

As chamadas também têm suporte nativo no [Obsidian Publish](https://help.obsidian.md/Obsidian+Publish/Introduction+to+Obsidian+Publish) .

Observação

Se você também estiver usando o plug-in Admonitions, atualize-o para pelo menos a versão 8.0.0 para evitar problemas com o novo recurso de texto explicativo.

### Alterar o título

Por padrão, o título da chamada é seu identificador de tipo em maiúsculas e minúsculas. Você pode alterá-lo adicionando texto após o identificador de tipo:

```markdown
> [!tip] Callouts can have custom titles
> Like this one.
```

> [!tip] Callouts can have custom titles
> Like this one.

Você pode até omitir o corpo para criar frases de destaque apenas de título:

```markdown
> [!tip] Title-only callout
```

> [!tip] Title-only callout

### Textos explicativos dobráveis

Você pode tornar uma chamada dobrável adicionando um sinal de mais (+) ou menos (-) diretamente após o identificador de tipo.

Um sinal de mais expande o texto explicativo por padrão e um sinal de menos o recolhe.

```markdown
> [!faq]- Are callouts foldable?
> Yes! In a foldable callout, the contents are hidden when the callout is collapsed.
```

> [!faq]- Are callouts foldable?
> Yes! In a foldable callout, the contents are hidden when the callout is collapsed.

### Chamadas aninhadas

Você pode aninhar textos explicativos em vários níveis.

```markdown
> [!question] Can callouts be nested?
> > [!todo] Yes!, they can.
> > > [!example]  You can even use multiple layers of nesting.
```

> [!question] Can callouts be nested?
> > [!todo] Yes!, they can.
> > > [!example]  You can even use multiple layers of nesting.

### Personalizar frases de destaque

[Snippets CSS](https://help.obsidian.md/Extending+Obsidian/CSS+snippets) e [plug-ins da comunidade](https://help.obsidian.md/Extending+Obsidian/Community+plugins) podem definir chamadas personalizadas ou até mesmo substituir a configuração padrão.

Para definir uma chamada personalizada, crie o seguinte bloco CSS:

```css
.callout[data-callout="custom-question-type"] {
    --callout-color: 0, 0, 0;
    --callout-icon: lucide-alert-circle;
}
```

O valor do `data-callout`atributo é o identificador de tipo que você deseja usar, por exemplo `[!custom-question-type]`.

- `--callout-color`define a cor de fundo usando números (0–255) para vermelho, verde e azul.
- `--callout-icon`pode ser um ID de ícone de [lucide.dev](https://lucide.dev/) ou um elemento SVG.

Nota sobre versões do ícone Lucide

Obsidian atualiza os ícones do Lucide periodicamente. A versão atual incluída é mostrada abaixo; use esses ícones ou anteriores em textos explicativos personalizados.  

Versão `0.268.0`  
Licença ISC  
Copyright (c) 2020, Lucide Contributors

Ícones SVG

Em vez de usar um ícone do Lucide, você também pode usar um elemento SVG como ícone de texto explicativo.

```css
--callout-icon: '<svg>...custom svg...</svg>';
```

### Tipos suportados

Você pode usar vários tipos de texto explicativo e aliases. Cada tipo vem com uma cor de fundo e um ícone diferentes.

Para usar esses estilos padrão, substitua `info`nos exemplos por qualquer um desses tipos, como `[!tip]`ou `[!warning]`.

A menos que você [Customize callouts](https://help.obsidian.md/Editing+and+formatting/Callouts#Customize%20callouts) , qualquer tipo não suportado será padronizado como o `note`tipo. O identificador de tipo não diferencia maiúsculas de minúsculas.

> [!note] Observação

```md
> [!note]
> Lorem ipsum dolor sit amet
```

---

> [!abstract] Abstrato

```md
> [!abstract]
> Lorem ipsum dolor sit amet
```

Apelido: `summary`,`tldr`

---

> [!info] Informações

```md
> [!info]
> Lorem ipsum dolor sit amet
```

---

> [!todo] Pendência

```md
> [!todo]
> Lorem ipsum dolor sit amet
```

---

> [!tip] Dica

```md
> [!tip]
> Lorem ipsum dolor sit amet
```

Apelido: `hint`,`important`

---

> [!success] Sucesso

```md
> [!success]
> Lorem ipsum dolor sit amet
```

Apelido: `check`,`done`

---

> [!question] Pergunta

```md
> [!question]
> Lorem ipsum dolor sit amet
```

Apelido: `help`,`faq`

---

> [!warning] Aviso

```md
> [!warning]
> Lorem ipsum dolor sit amet
```

Apelido: `caution`,`attention`

---

> [!failure] Falha

```md
> [!failure]
> Lorem ipsum dolor sit amet
```

Apelido: `fail`,`missing`

---

> [!danger] Perigo

```md
> [!danger]
> Lorem ipsum dolor sit amet
```

Alias:`error`

---

> [!bug] Erro

```md
> [!bug]
> Lorem ipsum dolor sit amet
```

---

> [!example] Exemplo

```md
> [!example]
> Lorem ipsum dolor sit amet
```

---

> [!quote] Citar

```md
> [!quote]
> Lorem ipsum dolor sit amet
```

Alias:`cite`

