---
tags:
  - TI/Programação/VSCODE
---
# Better Comments

A extensão Better Comments ajudará você a criar comentários mais amigáveis ​​no seu código.
Com esta extensão, você poderá categorizar suas anotações em:

- Alertas
- Consultas
- TODOs
- Destaques
- O código comentado também pode ser estilizado para deixar claro que o código não deveria estar ali
- Quaisquer outros estilos de comentário que você desejar podem ser especificados nas configurações

![[Pasted image 20241116202618.webp]]
## Configuração

Esta extensão pode ser configurada nas Configurações do usuário ou nas configurações do Workspace.

`"better-comments.multilineComments": true`
Esta configuração controlará se os comentários multilinha serão estilizados usando as tags de anotação. Quando falso, os comentários multilinha serão apresentados sem decoração.

`"better-comments.highlightPlainText": false`
Esta configuração controlará se os comentários em um arquivo de texto simples serão estilizados usando as tags de anotação. Quando verdadeiro, as tags (padrões: `! * ? //`) serão detectadas se forem o primeiro caractere em uma linha.

`better-comments.tags`
As tags são os caracteres ou sequências usadas para marcar um comentário para decoração. O padrão 5 pode ser modificado para alterar as cores, e mais podem ser adicionadas.

```json
"better-comments.tags": [  
{  
"tag": "!",  
"color": "#FF2D00",  
"strikethrough": false,  
"underline": false,  
"backgroundColor": "transparent",  
"bold": false,  
"italic": false  
},  
{  
"tag": "?",  
"color": "#3498DB",  
"strikethrough": false,  
"underline": false,  
"backgroundColor": "transparent",  
"bold": false,  
"italic": false  
},  
{  
"tag": "//",  
"color": "#474747",  
"strikethrough": true,  
"underline": false,  
"backgroundColor": "transparent",  
"bold": false,  
"italic": false  
},  
{  
"tag": "todo",  
"color": "#FF8C00",  
"strikethrough": false,  
"underline": false,  
"backgroundColor": "transparent",  
"bold": false,  
"italic": false  
},  
{  
"tag": "*",  
"color": "#98C379",  
"strikethrough": false,  
"underline": false,  
"backgroundColor": "transparent",  
"bold": false,  
"italic": false  
}  
]
```

## Idiomas suportados

- Ada
- AL
- Apex
- AsciiDoc
- BrightScript
- C
- C#
- C++
- ColdFusion
- Clojure
- COBOL
- CoffeeScript
- CSS
- Dart
- Dockerfile
- Elixir
- Elm
- Erlang
- F#
- Fortran
- gdscript
- GenStat
- Go
- GraphQL
- Groovy
- Haskell
- Haxe
- HiveQL
- HTML
- Java
- JavaScript
- JavaScript React
- JSON com comentários
- Julia
- Kotlin
- LaTex (inc. Bibtex/Biblatex)
- Menos
- Lisp
- Lua
- Makefile
- Markdown
- Nim
- MATLAB
- Objective-C
- Objective-C++
- Pascal
- Perl
- Perl 6
- PHP
- Pig
- PlantUML
- PL/SQL
- PowerShell
- Puppet
- Python
- R
- Racket
- ​​Ruby
- Rust
- SAS
- Sass
- Scala
- SCSS
- ShaderLab
- ShellScript
- SQL
- STATA
- Stylus
- Svelte
- Swift
- Tcl
- Terraform
- Twig
- TypeScript
- TypeScript React
- Verilog
- Visual Basic
- Vue.js
- XML
- YAML