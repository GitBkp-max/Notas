---
aliases: 
tags:
  - TI/Tecnologia/DBA/SQL
---
04/04/2024 10:40

[https://www.youtube.com/playlist?list=PLn_z5E4dh_LgWmEGn2lcdOp5TDKw6nkde](https://www.youtube.com/playlist?list=PLn_z5E4dh_LgWmEGn2lcdOp5TDKw6nkde "https://www.youtube.com/playlist?list=PLn_z5E4dh_LgWmEGn2lcdOp5TDKw6nkde")

# [[Linguagem SQL]] for absolute begginers

## Introdução

- SQL - Structure Query Language
- Linguagem padrão para acessar as informações em banco de dados
- SQL-ANSI/ISO: SQL padrão, funciona em praticamente  qualquer bando de dados (inclusive em arquiteturas de Big Data)

**Comandos são muito parecidos com a língua inglesa:**

```
SELECT banda, nome
FROM albuns
WHERE ano = 1969
```

![0f09ba894467928d6a07e69927971cdd.png](file:///C:/Users/marcos_alexandre/.config/joplin-desktop/resources/66c721a6ac574471a295e219e2a504ac.png)

**Tabelas (ou relações):** conjunto que representa m conceito do mundo real..

![8aa033e148d310fe82b10c91cb835731.png](file:///C:/Users/marcos_alexandre/.config/joplin-desktop/resources/fb1910fa5faa44b2a9ced320b1ccd4ee.png)

**Linhas (ou tuplas):** um exemplo especifico de uma tabela (observação)

![c9a8e0191504b7ca6f3b46fccc7f0868.png](file:///C:/Users/marcos_alexandre/.config/joplin-desktop/resources/6079e26fca00481fb62525d0f14f0b12.png)

**Colunas (ou atributos):** cada característica de uma tabela.

![2a5fa96459e7c375390889e3179e482d.png](file:///C:/Users/marcos_alexandre/.config/joplin-desktop/resources/0c690143d6414a2c8c39351edcb30fcb.png)

Domínio de um atributo: tipo da característica

![f9d108d2d942c1aaf91ffd53c762c1a3.png](file:///C:/Users/marcos_alexandre/.config/joplin-desktop/resources/71ddd4b94b6346ba9aa6cc624c110d77.png)

---

Construção de consultas?

1. Em que tabelas estão os dados necessários?
    
    - `FROM albuns`
2. Você precisa de todas as linhas ou precisa de um filtro?
    
    - `WHERE ano = 1969`
3. Você precisa de todas as colunas ou somente de algumas?
    
    - `SELECT banda, nome`

**DICA**  
"e se minha tabela tem 19283106 colunas? Tenho que colocar uma a uma no SELECT?"  
Para escolher todas as colunas da tabela na consulta, basta usar um asterisco (*)  
`SELECT *`

---

## Ordenação

Depois da consulta finalizada, podemos querer que seja ordenada por valores de uma ou mais colunas

O comando para realizar a ordenação é o **ORDER BY**

```
SELECT *
FROM bandas
WHERE Ano_Formacao = 1969
ORDER BY Ano_Formacao
```

---

## Agregação

Um conceito muito importante! agregar varias linhas por algumas colunas (ex.: bandas por estilos) é algo extremamente útil para entender melhor dos dados

O conceito muda um pouco, porque até então trabalhamos com linhas sem agrupamento

Utilizamos a agregação quanto queremos conhecer alguns dados de estatística descritiva dos dados: contagem, media, desvio padrao, minimo, maximo, etc

Estatística descritiva é somente uma forma de ter um conhecimento geral dos dados. Nesse curso vamos só ver algumas agregações de contagem, média. è o Famoso "big picture".

Mesmo sendo simples é um passo importante para ter um bom conhecimento dos dados que estão sendo trabalhados. Lembramos sempre que o foco da ciência de dados é a **SOLUÇÃO DE PROBLEMAS REIAS**

**![7ff91e34f41046d7b20952ffff9c74f3.png](file:///C:/Users/marcos_alexandre/.config/joplin-desktop/resources/795890d60f9049af962c4de75f971bc7.png)**

Exemplos de analises dos dados podemos fazer com agregações:

- Quantas bandas por estilo?
- Qual a média de álbuns por estilo?
- Quais estilos possuem média de álbuns acima de 10?
- Quais estilos possuem mais de 1 banda com ano de formação antes de 1970

Pense em perguntas de negocio a serem respondidas no seu dia a dia, quantos clientes por estado, média de vendas por colaborador, etc..

Em SQL, para agregar linhas utilizamos o comando **GROUP BY**

```
SELECT estilo, count(nome)
FROM bandas
GROUP BY estilo
```

Quantas bandas por estilo? (agregar por estilo)

![d8b7197905c1349b2ca3761f42bbe89f.png](file:///C:/Users/marcos_alexandre/.config/joplin-desktop/resources/d792c9826c8b4dadbef89d23e8ba6e14.png)

A agregação vai **COLAPSAR** as linhas por estilo

![3fb9e39123914ca7246cfd338fecba38.png](file:///C:/Users/marcos_alexandre/.config/joplin-desktop/resources/f7767f7203174c33a79157de14e879c7.png)

Como fazer para contar?? Vamos usar a função **COUNT**

```
SELECT estilo, count(nome)
FROM bandas
GROUP BY estilo
```

![6fd5ef03465e9b7d31ffeb9ea52d9f0e.png](file:///C:/Users/marcos_alexandre/.config/joplin-desktop/resources/4ee1a2ce2e634f189df78a0e1224cad1.png)

---

Qual a média de álbuns por estilo?

Para fazer a media vamos usar a função **AVG**

![5bb1f7f7a22da7a26903596135dde86c.png](file:///C:/Users/marcos_alexandre/.config/joplin-desktop/resources/3eca69991fdc48fa920e697fd236b880.png)

```
SELECT estilo, avg(Albuns)
FROM bandas
GROUP BY estilo
```

![ad572767c03c17658dc054117ade00fc.png](file:///C:/Users/marcos_alexandre/.config/joplin-desktop/resources/507f490d29b74fa58f3c9a614f5f97d1.png)

---

Filtros de linha já aprendemos! Usamos o WHERE

Mas Quando precisamos filtrar linhas por resultados de funções de agregação (count, avg, max, min, ...) utilizamos outro comando, o **HAVING**

Quais estilos possuem média de álbuns acima de 10? (agregar por estilo, calcular a média e filtrar com HAVING)

![eb5bd956c3b57e894ed718ab6fdc26ea.png](file:///C:/Users/marcos_alexandre/.config/joplin-desktop/resources/06f493e1ca2a48599017526dcb35ae60.png)

```
SELECT estilo, AVG(Albuns)
FROM bandas
GROUP BY estilo
HAVING AVG(Albuns)>10
```

![1c93d641fd454e80376e4c2aa0b68ee9.png](file:///C:/Users/marcos_alexandre/.config/joplin-desktop/resources/5b8b37460c5b495cb271e3cd6970442e.png)

Podemos usar o WHERE e o HAVING numa mesma consulta! Enquanto o WHERE filtra por valores não agregados, o HAVING filtra por valores agregados

---

Quais estilos possuem mais de 1 banda com ano de formação antes de 1970? (agregar por estilo, contar as bandas por estilo, filtrar linhas por ano de formação e finalmente filtrar pela quantidade de bandas)

```
SELECT estilo, COUNT(nome)
FROM bandas
WHERE Ano_Formacao<1970
GROUP BY estilo
HAVING COUNT(nome)>1
```

![db3c61f5e5bbe2a368d52cacbe7a260f.png](file:///C:/Users/marcos_alexandre/.config/joplin-desktop/resources/44878304b041415ab4a582a7cd131ddf.png)

---

# Consultas elaboradas

Vai ser capaz de responder perguntas de negocio utilizando SQL:

- Com múltiplos filtros (ex: clientes de SP ou RJ com idade acima de 30)
- Com cálculos matematicos e estatisticos (ex: média de idade de uma turma)
- Com filtros aproximados (ex: pacientes que contenham João no nome)
- Com múltiplas agregações (ex: agrupar produtos por fornecedores e transportadores)

## Múltiplos filtros

Na vida de cientistas de dados será MUITO comum você ter que criar filtros diferentes para analisar os dados, buscar insights e responder algumas perguntas de negocio

A gente já viu como fazer um filtro para linhas sem agregação com o WHERE

Não vimos muitos exemplos, no entanto, de múltiplos filtros! Vamos fazer isso agora! Para combinar critérios de filtros utilizamos as palavras-chave AND e OR

![fac60c27385c4afb7775c664dfdb6c35.png](file:///C:/Users/marcos_alexandre/.config/joplin-desktop/resources/cbe08d28b1c24cc5b3cb550c551c1b27.png)

Ex. CPf e nome dos clientes do sexo feminino com idade acima de 20 anos

1. Em que tabela estão os dados necessários?
    - Tabela Clientes!
    - `SELECT * FROM clientes</span>`
2. Você precisa de todas as linhas ou precisa de um filtro?
    - Dois filtros: sexo e idade!
    - `SELECT * FROM clientes WHERE sexo = 'F' AND idade >20`
3. Você precisa de todas as colunas ou somente de algumas?
    - Só duas colunas: cpf e nome!
    - `SELECT cpf, nome FROM clientes WHERE sexo = 'F' AND idade >20`

![e4565a1ac0508c1241ce417d669d2f3b.png](file:///C:/Users/marcos_alexandre/.config/joplin-desktop/resources/048ba3284c534900aa4420fde6d9a432.png)

Ex. Nome e idade dos clientes de São Paulo ou Goiás com idade menor ou igual a 22

1. Em que tabela estão os dados necessários?
    - `SELECT * FROM clientes`
2. Você precisa de todas as linhas ou precisa de um filtro?
    - `SELECT * FROM cleites WHERE (uf = 'SP' OR uf = 'GO') AND idade <=22`
3. Você precisa de todas as colunas ou soemte de algumas?
    - `Select nome, idade FROM clientes WHERE (uf='SP' OR uf='GO') AND idade <= 22`

![bc1b8ae1b6fcb9f66829eb61fb3c98ab.png](file:///C:/Users/marcos_alexandre/.config/joplin-desktop/resources/46b14efc9c4c41d7b5247bd563988f68.png)

## Filtros aproximados

Muitos das consultas que fizemos no dia a dia requerem filtros que não seja, exatos! filtros aproximados são uteis para buscar informações importantes dentro de bancos de dados exemplos

- Selecionar clientes que tenham "madalena" no endereço?
- Selecionar pacientes que tenha a 2a Letra igual a "a"?
- Selecionar alunos com nome começando com M ou L?

### Caracteres Coringa

- % - substitui qualquer caractere em qualquer quantidade
- _ - substitui um caractere(somente um)
- [..] - alternativas, qualquer um dentro dos colchetes

Ex Selecionar nomes de clientes que tenham 'Madalena' no endereço

![3b12057566da40ecffd49a3b8e89d119.png](file:///C:/Users/marcos_alexandre/.config/joplin-desktop/resources/e88d8bce9622429bbb8b442d3787a7f7.png)

```
SELECT nome
FROM clientes
WHERE endereco LIKE '%Madalena%'
```

![0a1c328e0aec4979ff74cebb363b097d.png](file:///C:/Users/marcos_alexandre/.config/joplin-desktop/resources/df9ad8580b4641288c32ad639d48527b.png)

Ex Selecionar nome e cpf de alunos com 2a letra igual a 'o'

![495825e552be9aac43d47b6ff3051199.png](file:///C:/Users/marcos_alexandre/.config/joplin-desktop/resources/cbdba3517b154b2c8184123ce2cbea52.png)

```
SELECT *
FROM alunos
WHERE nome LIKE '_o%'
```

![d103096043232cb87eb59dc7158a38cd.png](file:///C:/Users/marcos_alexandre/.config/joplin-desktop/resources/26f11d1159f14dc2b1e09a5b2e144402.png)

EX Selecionar o nome e idade de alunos começando com M ou L

![495825e552be9aac43d47b6ff3051199.png](file:///C:/Users/marcos_alexandre/.config/joplin-desktop/resources/cbdba3517b154b2c8184123ce2cbea52.png)

```
SELECT * 
FROM alunos
WHERE nome LIKE '[ML]%'
```

---

## Funções Matemáticas

Na parte básica falamos de algumas funções somente em exemplos de agregação  
Agora que ja entendemos bem a agregação, podemos conhecer melhor funções matemáticas e de estatística descritiva sem precisar utilizar o GROUP BY

A s funções mais utilizadas são: **COUNT, AVG, MAX, MIN, STDEV**  
Com essas funções nosso leque de ferramentas melhora bastante no caminho para entender melhor nossos dados

---

**COUNT**: conta o numero de linhas ou os valores não nulos  
Se fizer um **SELECT COUNT(*)**, ele vai retornar a contagem de linhas. Todas as linhas Independentes de ter valores nulos.

_"e se eu quiser o numero de valores sem repetição"_  
Pode se contar valores únicos utilizando o **DISTINCT**   
O **DISTINC** é usado para retornar valores únicos, sem repetição. na contagem temos que passar a palavra-chave DENTRO do COUNT

![f19c7545cace089c4feef066c079f195.png](file:///C:/Users/marcos_alexandre/.config/joplin-desktop/resources/b3a0064264744175a349602b8475d54e.png)

---

**AVG**: Calcula a media da coluna selecionada  
o AVG vai ignorar valores NULOS e desconsidera-los na hora de calcular a media

---

**MAX** e **MIN** nem precisa explicar:

![296570fc4684ab311e0e1fb5e577cc10.png](file:///C:/Users/marcos_alexandre/.config/joplin-desktop/resources/99055f8fede941a8b22045c90cc13af7.png) ![46297fef884d16b4aae22c0b20e56e6e.png](file:///C:/Users/marcos_alexandre/.config/joplin-desktop/resources/333786fe3e994068a5998c9877a419d1.png)

---

**STEDV**: Calcula o desvio padrão da coluna selecionada  
Conceito de estatística que apresenta o quão variados e dispersos estão so dados

![83710cf7d1323602c08d0c0cb8756818.png](file:///C:/Users/marcos_alexandre/.config/joplin-desktop/resources/84418e5ea43e41708e676f5565b4bd22.png) ![95bc6b34d74d8b6408d7302f81ad0c41.png](file:///C:/Users/marcos_alexandre/.config/joplin-desktop/resources/054a6564d77c4577a63e2e367a7167f2.png)