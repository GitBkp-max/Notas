---
tags:
  - Tecnologia/DBA
aliases:
---
02/04/2024 11:33  
[Aula usado para o material](https://www.youtube.com/watch?v=2oFepn1KXQM "https://www.youtube.com/watch?v=2oFepn1KXQM")

# SGBD - Sistema Gerenciador de Banco de Dados

_Programas utilizados para gerenciar a estrutura e as informações dos Bancos de Dados_

**As funções incluem:**

- Transformar e apresentar dados;
- Controlar acesso de multiusuário;
- Prover interfaces de comunicação do banco de dados;

Os programas usados em um SGBD permite criação de estruturas, manutenção de dados, gerenciamento de transações e extração de informações

![SGBD.drawio.png](file:///C:/Users/marcos_alexandre/.config/joplin-desktop/resources/5b8a048ea26b43dd82d091ed0d6c0177.png)

## VISÂO

É o conceito que permite que os diferentes usuários compartilhem dados e recursos de processamento

![9201dca326149e0f55c7aa46dadd617e.png](file:///C:/Users/marcos_alexandre/.config/joplin-desktop/resources/385521532bf547e383fbe62eb7b1ee95.png)![a93f582c89d017efde40371e606db2cf.png](file:///C:/Users/marcos_alexandre/.config/joplin-desktop/resources/a888cb955e7e4024a6a5cc4e10c6cff8.png)

## Relacional

_Propriedade Básica da Transação_

### [[ACID]]

- Atomicidade
    - Transação deve ter todas suas operações executadas em caso de sucesso
    - Apresentando falhas nenhum resultado das operações deve refletir sobre o banco de dados
- consistência
    - Respeitar as regras de integridade dos dados
    - A execução de uma transação leva o banco de dados de um estado consistente para outro estado consistente
- isolamento
    - Evitar que transações paralelas interfiram umas nas outras
- durabilidade
    - Persistências dos efeitos de uma transação no banco de dados em caso de sucesso, mesmo com quedas de energia, erros ou travamentos

![c50d122037c000e1a9ec3adb0569caa8.png](file:///C:/Users/marcos_alexandre/.config/joplin-desktop/resources/86263b5879664ea0ad88440f3a88d673.png)

|                                                                                                                                                  |                                                                                       |
| ------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------- |
| **SGBDR**                                                                                                                                        | **SGBD NoSQL**                                                                        |
| - Microsoft SQL Server<br>- [[Artigos/MySQL]]<br>- PostgreSQL<br>- Oracle DATABASE<br>- Firebird<br>- Microsoft ACCESS<br>- LibreOffrice BASE<br>- dBase | - MongoDB<br>- Redis<br>- Cassandra<br>- Neo4j<br>- Amazon DynamoDB<br>- Apache HBase |

---

## [[NoSQL]]

Classificação dos modelos existem em NoSQL de acordo com a estrutura que os dados são armazenados

**Atualmente, existem 4 modelos principais**

- Orientado a chave-valor
- orientado a documentos
- orientado a colunas
- orientado a grafos

|   |   |   |   |
|---|---|---|---|
|**Chave Valor**|**Documentos**|**Colunas**|**Grafos**|
|- DynamoDB<br>- Redis<br>- Riak<br>- Memcached<br>- Berkeley DB<br>- LevelDB|- MongoDB<br>- Couchbase<br>- CounchDB<br>- MarkLogic<br>- RavenDB|- Cassandra<br>- Hbase<br>- Hypertable|- AllegroGraph<br>- RangoDB<br>- InfoGrid<br>- Neo4j<br>- Titan<br>- FlockDB<br>- HyperGraphDB|

### Teorema CAP

_Outro aspecto em aos bancos de dados NoSQL é o teorema CAP, proposto em 2000 pelo pesquisador Eric Brewer_

**Consiste em um conjunto de requisitos para sistemas distribuídos:**

- **Consistência** (Consistency)
    - è preciso garantir que todos os servidores de um cluster terão copias consistentes dos dados
- **Disponibilidade** (Availability)
    - Para o requisito o sistema devera sempre responder a uma requisição, mesmo que não esteja consistente
- **Tolerância a partição** (Partition tolerance)
    - A tolerancia a partição deve garantir que o sistema continuara em operação mesmo que algum servidor do cluster venha falhar

![TeoremaCAP.png](file:///C:/Users/marcos_alexandre/.config/joplin-desktop/resources/1408d50b0e3747a3acf9b356df14ab78.png)

Ainda segundo Brewe, é teoricamente impossível obter um sistema que atenda os 3 requisitos.  
Segundo o teorema CAP, se você precisar garantir consistência e disponibilidade para uma aplicação, você precisara abrir mão da tôlerancia a partição, pois ele não oferece garantias em relação a alta consistência dos dados se precisar manter a aplicação sempre disponivel

---