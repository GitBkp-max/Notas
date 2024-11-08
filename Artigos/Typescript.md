---
tags:
  - Programação/TS
  - Artigos
---
O TypeScript é uma linguagem **superset** do JavaScript, ou seja, tudo o que você pode fazer em JavaScript, também pode ser feito em TypeScript. A principal diferença é que o TypeScript adiciona **tipagem estática** e **recursos avançados de desenvolvimento**, como interfaces e classes, que ajudam a criar código mais seguro e escalável.
Vamos percorrer do básico ao avançado:

# 1. **Básico**

## a. **Tipos Primitivos**

O TypeScript tem tipagem explícita, ou seja, você pode (e deve) definir o tipo de variáveis.

```ts
typescript
Copiar código
let nome: string = "João";
let idade: number = 25;
let ativo: boolean = true;

```

Se você tentar atribuir um valor de tipo diferente, o TypeScript vai acusar erro:

```ts
typescript
Copiar código
nome = 42; // Erro: Type 'number' is not assignable to type 'string'

```

## b. **Tipos Implícitos**

Se você não declarar um tipo, o TypeScript vai inferir o tipo com base no valor inicial.

```ts
typescript
Copiar código
let mensagem = "Olá"; // Infere que mensagem é do tipo string
mensagem = 123; // Erro

```

## c. **Funções**

No TypeScript, você pode declarar o tipo de retorno e o tipo dos parâmetros de uma função.

```ts
typescript
Copiar código
function somar(a: number, b: number): number {
  return a + b;
}

```

Se você tentar retornar um valor que não é do tipo especificado, o TypeScript avisará.

# 2. **Intermediário**

## a. **Interfaces**

As interfaces permitem descrever a estrutura de objetos, garantindo que eles sigam um padrão.

```ts
typescript
Copiar código
interface Usuario {
  nome: string;
  idade: number;
  ativo: boolean;
}

let usuario: Usuario = {
  nome: "Maria",
  idade: 30,
  ativo: true,
};

```

## b. **Tipos opcionais e padrões**

Você pode definir parâmetros opcionais e valores padrão em funções e objetos.

```ts
typescript
Copiar código
function apresentar(nome: string, idade?: number): string {
  return idade ? `${nome} tem ${idade} anos` : `${nome} não informou a idade`;
}

```

Aqui, `idade` é opcional.

## c. **Union Types**

Você pode definir que uma variável pode assumir mais de um tipo usando o pipe (`|`).

```ts
typescript
Copiar código
let valor: string | number;
valor = "Texto";
valor = 123;

```

# 3. **Avançado**

## a. **Generics**

Generics permitem criar componentes que podem trabalhar com diferentes tipos, mantendo a segurança de tipo.

```ts
typescript
Copiar código
function identidade<T>(valor: T): T {
  return valor;
}

console.log(identidade<string>("Teste"));
console.log(identidade<number>(123));

```

Aqui, `T` é um tipo genérico, que pode ser substituído por qualquer tipo quando a função é chamada.

## b. **Tipos Avançados**

Você pode combinar tipos para criar tipos mais complexos.

- **Intersection Types**: permite combinar múltiplos tipos.

```ts
typescript
Copiar código
interface Pessoa {
  nome: string;
}

interface Funcionario {
  salario: number;
}

type Empregado = Pessoa & Funcionario;

let empregado: Empregado = { nome: "Lucas", salario: 3000 };

```

- **Mapped Types**: permite modificar todos os campos de um tipo.

```ts
typescript
Copiar código
type Opcional<T> = {
  [K in keyof T]?: T[K];
};

type UsuarioOpcional = Opcional<Usuario>;

```

Aqui, `UsuarioOpcional` é uma versão de `Usuario` com todos os campos opcionais.

## c. **Decorators**

Decorators são uma funcionalidade experimental do TypeScript que permite adicionar metadados ou modificar classes e suas propriedades de maneira declarativa.

```ts
typescript
Copiar código
function log(constructor: Function) {
  console.log("Classe criada: ", constructor.name);
}

@log
class Carro {
  constructor(public marca: string) {}
}

let fusca = new Carro("Volkswagen");

```

Aqui, o decorator `@log` será executado quando a classe `Carro` for criada.

## d. **Modules**

Assim como o JavaScript moderno, o TypeScript suporta **módulos**. Um módulo exporta e importa partes de código.

```ts
typescript
Copiar código
// arquivo math.ts
export function somar(a: number, b: number): number {
  return a + b;
}

// arquivo app.ts
import { somar } from './math';
console.log(somar(2, 3)); // 5

```

# Ferramentas de Compilação e Execução

- **Compilador TypeScript**: O comando `tsc` (TypeScript Compiler) converte TypeScript em JavaScript.

```bash
bash
Copiar código
tsc arquivo.ts
```

- **ts-node**: Uma ferramenta que permite executar arquivos TypeScript diretamente, sem a necessidade de compilar manualmente.

```bash
bash
Copiar código
ts-node arquivo.ts
```


# Conclusão

O TypeScript oferece uma poderosa tipagem estática e recursos como interfaces, tipos avançados, decorators e generics, que aumentam a escalabilidade e robustez do código. Ele é altamente recomendado para projetos grandes e quando se deseja maior controle sobre o código JavaScript.

Se precisar de mais detalhes em qualquer parte, posso aprofundar!