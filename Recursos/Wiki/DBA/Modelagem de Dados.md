---
aliases: 
tags:
  - Tecnologia/DBA
---
02/04/2024 19:06  
[Aula usada para esse material](https://www.youtube.com/playlist?list=PLLbQNZHBOJtaK4vGBmdvOaPaxDJxRT0fe "https://www.youtube.com/playlist?list=PLLbQNZHBOJtaK4vGBmdvOaPaxDJxRT0fe")

# Modelagem de dados

## O que é Modelagem de Dados

Modelagem de dados é uma técnica essencial no campo da gestão de informações, utilizada para especificar as regras e estruturas que definem como os dados serão organizados, armazenados e manipulados em um banco de dados. Consiste em um processo detalhado de design, onde são identificados e definidos os tipos de dados, relacionamentos entre eles, restrições de integridade e outras características necessárias para garantir a eficiência e consistência dos dados ao longo do tempo.

Existem diferentes abordagens e metodologias para a modelagem de dados, cada uma com suas próprias técnicas e ferramentas. Entre as mais comuns estão o modelo entidade-relacionamento (ER), o modelo relacional, o modelo dimensional e o modelo de objeto. Cada um desses modelos é adequado para diferentes contextos e requisitos de negócios.

A modelagem de dados desempenha um papel crucial em projetos de desenvolvimento de software, sistemas de informação e análise de dados, pois ajuda a garantir que os dados sejam organizados de forma lógica e eficiente, facilitando a consulta, análise e tomada de decisões. Além disso, uma modelagem de dados bem elaborada pode contribuir significativamente para a integridade, segurança e escalabilidade de um sistema de banco de dados.

---

## O que é um banco de dados

è um repositório de informações, é uma forma de armazenar informações de maneira sistêmica, em um sistema de computador, essas informações que são armazenadas precisam se relacionar conosco para isso existe o banco de dados **relacional** e o **não relacional** (mencionado no introdução ao SGBD)

### Elementos do banco de dados

Um banco de dados é composto por diversos elementos essenciais que trabalham em conjunto para armazenar, gerenciar e recuperar informações de forma eficiente. Entre os principais elementos, destacam-se:

- **Arquivos**: São os locais físicos onde os dados são armazenados. Os dados dentro de um banco de dados são organizados em arquivos para facilitar o acesso e a manipulação.
    
- **Sistema de Gerenciamento de Banco de Dados (SGBD)**: É o software responsável por gerenciar o banco de dados como um todo. Ele oferece uma interface para criar, manipular e consultar dados, além de garantir a integridade, segurança e eficiência do banco de dados.
    
- **Linguagem de Consulta Estruturada (SQL)**: É a linguagem padrão para comunicação com bancos de dados relacionais. Com o SQL, os usuários podem realizar consultas para recuperar, inserir, atualizar e excluir dados de maneira fácil e eficiente.
    

Esses elementos formam a base de um banco de dados, permitindo que as organizações armazenem e gerenciem grandes volumes de dados de forma organizada e acessível.

---

## Conceitos do banco de dados

- **Entidade de dados**
    - E um conjunto representação do mundo real do que precisa ser armazenado em um bando de dados
    
    - As entidades devem ser únicas dentro de um mesmo _schema_ de dados
    - Uma entidade representa um objeto do mundo real ou conceitual, sobre o qual se deseja armazenar informações. Por exemplo, em um sistema de gerenciamento de biblioteca, as entidades podem incluir Livro, Autor e Usuário.
- **Atributo**
    - É aquilo que caracteriza uma entidade. As informações da entidade que serão armazenadas
    
    - Um atributo é uma propriedade ou característica de uma entidade que descreve aspectos específicos dessa entidade. Por exemplo, para a entidade Livro, os atributos podem ser Título, Autor, Ano de Publicação e ISBN.
- **Tuplas**
    - São basicamente as linhas dessa tabela com os valores dos atributos
    
    - Uma tupla é uma coleção ordenada de valores que representam uma instância específica de uma entidade em um banco de dados. Cada valor na tupla corresponde a um atributo da entidade. Por exemplo, uma tupla na entidade Livro pode ser ("O Senhor dos Anéis", "J.R.R. Tolkien", 1954, "978-0-395-19395-6").
- **Relacionamento entre entidades**
    
    - representa a associação que podemos ter entre duas ou mais entidades
- **Cardinalidades**
    - é um conceito que determina quantos itens podem se relacionar entre as entidades
    - A cardinalidade descreve o número de ocorrências que uma entidade pode ter em relação a outra entidade em um relacionamento. Existem três tipos principais de cardinalidade: um-para-um (1:1), um-para-muitos (1:N) e muitos-para-muitos (M:N). Por exemplo, em um sistema de banco de dados de uma escola, um professor pode ter muitos alunos (1:N), mas um aluno só pode ter um professor (N:1).
        - **Um-para-um (1:1)**: Neste tipo de relacionamento, uma instância de uma entidade está associada a no máximo uma instância de outra entidade, e vice-versa. Por exemplo, em um sistema de gerenciamento de funcionários, cada funcionário pode ter apenas um chefe e cada chefe pode ter apenas um funcionário direto.
        - **Um-para-muitos (1:N)**: Este é o tipo mais comum de relacionamento, onde uma instância de uma entidade está associada a zero ou mais instâncias de outra entidade. Por exemplo, em um sistema de gerenciamento de pedidos online, um cliente pode fazer vários pedidos, mas cada pedido pertence a apenas um cliente.
        - **Muitos-para-muitos (M:N)**: Neste tipo de relacionamento, várias instâncias de uma entidade estão associadas a várias instâncias de outra entidade. Esse tipo de relacionamento requer uma tabela de associação (ou tabela de junção) para mapear as conexões entre as duas entidades. Por exemplo, em um sistema de gerenciamento de estudantes e disciplinas, um estudante pode se matricular em várias disciplinas, e cada disciplina pode ter vários estudantes matriculados.
        - **Auto-relacionamento**: Em um auto-relacionamento, uma entidade se relaciona consigo mesma. Isso pode acontecer quando uma instância da entidade se relaciona com outra instância da mesma entidade. Por exemplo, em uma hierarquia de funcionários, cada funcionário pode ter um supervisor que é ele mesmo um funcionário.
- **Chaves**
    - **Chaves primarias**
        - Primay Key (PK) : é a principal chave de uma entidade, identificando de forma única e exclusiva um registro
        - não pode ter repetição e nem ser nula
        
    - **Chaves estrangeiras**
        - Foreign Key (FK): determina o relacionamento entre duas tabelas
        - consegue saber qual atributo consegue se relacionar
        
    - Uma chave é um conjunto de um ou mais atributos que podem ser usados para identificar exclusivamente uma tupla em uma entidade. Existem diferentes tipos de chaves, incluindo chave primária, chave candidata, chave estrangeira, etc. A chave primária é a chave principal usada para identificar exclusivamente cada tupla em uma tabela. Por exemplo, em uma tabela de Alunos, o número de matrícula pode ser a chave primária, garantindo que cada aluno seja identificado de forma única.

---

## Análise de Requisitos

Requisitos referem-se a todas as necessidades, especificações e condições que devem ser atendidas ou cumpridas em um projeto, produto ou sistema. Eles são essenciais para orientar o desenvolvimento e garantir que o resultado final atenda às expectativas e às necessidades dos usuários finais. Durante a análise de requisitos, é fundamental entender:

- **O que**:
    
    - Refere-se às funcionalidades, características e dados que o sistema deve incluir ou manipular. Isso envolve identificar os recursos específicos que serão oferecidos pelo sistema para atender às necessidades dos usuários.
- **Por que**:
    
    - É importante compreender os motivos e as justificativas por trás de cada requisito. Isso ajuda a priorizar e a tomar decisões informadas durante o processo de desenvolvimento, garantindo que cada requisito contribua para os objetivos gerais do projeto.
- **Como**:
    
    - Envolve determinar a melhor maneira de implementar e realizar cada requisito. Isso inclui considerar as tecnologias, as metodologias e as práticas de desenvolvimento mais adequadas para atender às necessidades identificadas.
- **Para que**:
    
    - Refere-se aos objetivos e às metas que cada requisito visa alcançar. Compreender o propósito de cada requisito ajuda a garantir que o sistema desenvolvido seja relevante, útil e eficaz para os usuários finais.
- **Quanto**:
    
    - Envolve estimar recursos, como tempo, custo e esforço, necessários para implementar cada requisito. Isso é crucial para planejar adequadamente o projeto, gerenciar expectativas e garantir a viabilidade do desenvolvimento.

Ao abordar essas questões durante a análise de requisitos, é possível garantir que o sistema desenvolvido atenda não apenas às expectativas dos usuários finais, mas também aos objetivos e às restrições do projeto como um todo.

---

## Modelagem conceitual

A modelagem conceitual é uma etapa importante no desenvolvimento de sistemas de informação e envolve a criação de representações abstratas dos requisitos do sistema, independentemente de qualquer tecnologia específica. Geralmente, essa modelagem é realizada por meio de diagramas e outras ferramentas que ajudam a visualizar e compreender os elementos-chave do sistema.

Um dos métodos mais comuns para realizar a modelagem conceitual é o uso de diagramas de entidade-relacionamento (ER), que descrevem as entidades do sistema, os atributos dessas entidades e os relacionamentos entre elas. Outras técnicas, como diagramas de classes UML (Unified Modeling Language), também podem ser utilizadas, especialmente em sistemas orientados a objetos.

A modelagem conceitual ajuda os desenvolvedores e os stakeholders a entenderem os requisitos do sistema de forma clara e concisa, além de fornecer uma base sólida para o desenvolvimento posterior do sistema. Ela também facilita a comunicação entre as diferentes partes interessadas no projeto, garantindo que todos tenham uma compreensão comum dos objetivos e funcionalidades do sistema a ser desenvolvido.

### **Processo de Modelagem Conceitual:**

1. **Compreensão das Entidades:**
    
    - Primeiramente, é essencial compreender as entidades envolvidas no sistema. Por exemplo:
        - Aula
        - Professor
2. **Identificação dos Relacionamentos:**
    
    - Em seguida, devemos identificar os relacionamentos entre as entidades. Por exemplo:
        - Relacionamento "Leciona" entre Professor e Aula
3. **Determinação das Cardinalidades:**
    
    - Para cada relacionamento, é necessário determinar as cardinalidades para indicar a quantidade de instâncias de uma entidade que podem estar associadas a uma instância da outra. Exemplo:
        - Cardinalidade [1..N] para o relacionamento "Leciona", indicando que um professor pode lecionar várias aulas.
4. **Identificação dos Atributos:**
    
    - Posteriormente, devemos identificar os atributos específicos de cada entidade. Por exemplo:
        - Para Professor:
            - ID (Identificador)
            - Nome
            - Data de Nascimento
        - Para Aula:
            - ID (Identificador)
            - Descrição
            - Carga Horária
5. **Classificação dos Atributos:**
    
    - Na modelagem conceitual, os atributos podem ser classificados em quatro tipos:
        1. **Identificador**: Atributo que identifica exclusivamente cada registro (representado por bola preenchida).
        2. **Não Identificador**: Atributos comuns (representados por bola vazia).
        3. **Multivalorado**: Atributo que pode conter vários valores (representado por asteriscos).
        4. **Composto**: Atributo que pode conter sub-atributos (representado por um símbolo oval).

Seguir esses passos proporciona uma modelagem conceitual clara e abrangente, fornecendo uma base sólida para o desenvolvimento subsequente do sistema.

---

## **Modelagem Lógica:**

_**Transformando Conceitos em Estruturas de Dados**_

A modelagem lógica representa a etapa intermediária entre os conceitos abstratos definidos na modelagem conceitual e sua implementação física em um banco de dados ou sistema de informação. Essa fase é crucial para garantir uma transição suave do conceitual para o físico, proporcionando uma base sólida para a construção do sistema. Aqui estão os principais elementos e passos envolvidos:

**Elementos para ser utilizados**

- Entidades: Representam os principais objetos ou conceitos do sistema.
- Atributos: Características ou propriedades das entidades.
- Cardinalidade: Define a relação entre entidades, indicando quantos objetos de uma entidade estão associados a um objeto da outra.
- Definição de Chaves: Identificação dos atributos que servirão como chaves primárias e estrangeiras para garantir a integridade dos dados.

Ao seguir esses elementos e passos, é possível desenvolver uma modelagem lógica robusta e eficaz, que servirá como alicerce para a implementação prática do sistema de informação ou banco de dados.

---

## Modelagem Física

A modelagem física é a etapa final no projeto de banco de dados, onde o modelo lógico é traduzido em um esquema de banco de dados concreto, pronto para ser implementado em um sistema de gerenciamento de banco de dados (SGBD). Aqui estão os principais aspectos envolvidos na modelagem física

```
/* Modelagem de dados aula - logica: */

CREATE TABLE Professor (
    ID INTEGER PRIMARY KEY,
    Nome VARCHAR,
    Data_nasc DATE
);

CREATE TABLE Aula (
    ID INTEGER PRIMARY KEY,
    Descricao VARCHAR,
    Carga_horaria TIME,
    id_professor INTEGER
);
 
ALTER TABLE Aula ADD CONSTRAINT FK_Aula_2
    FOREIGN KEY (id_professor)
    REFERENCES Professor (ID);
```

---

# NORMALIZAÇÃO

## O que é?

A normalização é um conjunto de regras essenciais para proteger os dados em um banco de dados, tornando-o mais flexível, eliminando redundâncias de informações e minimizando os custos associados a mudanças. Essas regras garantem uma estrutura eficiente para organizar os dados, reduzindo a duplicidade e mantendo a integridade das informações. Ao seguir as práticas de normalização, podemos criar um ambiente de banco de dados mais robusto, adaptável e seguro.

## Primeira Forma Normal (1FN):

Garante que cada coluna de uma tabela contenha apenas valores atômicos, ou seja, valores indivisíveis.  
E também quando não contem atributos compostos e multivalorados

## Segunda Forma Normal (2FN):

Além de atender à 1FN, cada coluna não chave depende completamente da chave primária.  
Os atributos de cada tabela precisam ser independentes entre si.

## Terceira Forma Normal (3FN):

Além de atender à 2FN, não deve haver dependências transitivas entre as colunas não chave.

## Outras formas nominais posteriores:

- **Forma Normal de Boyce-Codd (BCNF):** Uma forma mais restrita da 3FN, na qual cada determinante de uma relação deve ser uma superchave candidata.
- **Quarta Forma Normal (4FN):** Elimina dependências multivaloradas e relações de muitos para muitos.
- **Quinta Forma Normal (5FN):** Elimina dependências de junção e de inserção.
