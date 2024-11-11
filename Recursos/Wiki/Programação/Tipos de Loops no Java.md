---
tags:
  - TI/Programação/Java
---
## Explorando os Tipos de Loops em Java!
Você já se perguntou qual loop usar para resolver um problema de forma mais eficiente? Hoje compartilho um pouco sobre os tipos de loops que você encontra em Java e em quais casos cada um é mais útil.

1. for loop
	- Útil quando sabemos o número de iterações.
	- Exemplo prático: iterar sobre uma lista de números para encontrar o maior valor.

Sintaxe: 
```java
for (int i = 0; i< 10; i++){
	System.out.println(i);
}
```

Aplicando:
```java
public class Exemplo {
	public static void main(String[] args){
		int soma = 0;
		
		for (int i = 1; <= 20; i++){
			//Verifica se o número é par
			if (i % 2 == 0){
				soma += i; // Adiciona o número par à soma
			}
		}
		System.out.print("A soma dos números pares de 1 a 20 é: " + soma);
	}
}
```

2. while loop
	- Ideal para quando não sabemos quantas vezes precisamos repetir, mas temos uma condição de parada.
	- Exemplo prático: processar entrada do usuário até que ele digite “sair”.

Sintaxe: 
```java
int x = 0;
while (x < 10){
	System.out.println(x);
	x++;
}
```

Aplicando:
```java
public class Exemplo {
	public static void main(String[] args){
		int soma = 0; // Variavel para armazenar a soma dos numeros pares
		int i = 1; // Variavel de controle, começando em 1
		
		//Loop que continua enquanto i for menor ou igual a 20
		while (i <= 20){
			// Verifica se o numero é par
			if (i % 2 == 0){
				soma += i; // Adiciona o numero à soma
			}
			i++; // Incrementa i a cada iteração
		}
		// Exibe o resultado final
		System.out.print("A soma dos números pares de 1 a 20 é: " + soma);
	}
}
```

3. do-while loop
	- Similar ao while, mas garante que o bloco será executado pelo menos uma vez, independentemente da condição.
	- Exemplo prático: mostrar um menu de opções até o usuário escolher uma opção válida.

Sintaxe: 
```java
int x = 0;
do {
	System.out.print(x);
	x++;
} while (x < 10);
```

Aplicando:
```java
public class Exemplo {
	public static void main(String[] args){
		int soma = 0; // Variavel para armazenar a soma dos numeros pares
		int i = 1; // Variavel de controle, começando em 1
		
		//Loop que garante a execução pelo menos uma vez
		do {
			// Verifica se o numero é par
			if (i % 2 == 0){
				soma += i; // Adiciona o numero à soma
			}
			i++; // Incrementa i a cada iteração
		} while (i <= 20); // Condição de continuidade do loop
		// Exibe o resultado final
		System.out.print("A soma dos números pares de 1 a 20 é: " + soma);
	}
}
```

4. for-each loop (Enhanced for)
	- Perfeito para iterar sobre arrays ou coleções de forma mais simples.
	- Exemplo prático: imprimir todos os itens de uma lista sem precisar de um índice.

Sintaxe: 
```java
int[] numbers = {1, 2, 3, 4};
for (int num : numbers){
	System.out.println(num);
}
```

Aplicando:
```java
public class Exemplo {
	public static void main(String[] args){
		List<Integer> numeros = new ArrayList<>();
		//Preenchendo a lista com os numeros de 1 a 20
		for (int i = 1; i <= 20; i++){
			numeros.add(i);
		}
		int soma = 0;
		// Usando for-each para somar os numeros pares
		for (int numero : numeros){
			if (numero % 2 == 0){
				soma += numero;
			}
		}
		// Exibe o resultado final
		System.out.println("A soma dos números pares de 1 a 20 é: " + soma);
	}
}
```
