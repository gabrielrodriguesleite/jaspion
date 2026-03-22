+++
title = 'Java Cheatsheet - Guia Rápido'
date = 2026-03-21T09:09:55-03:00
draft = false
+++

# Java Cheatsheet - Guia Rápido

## Sintaxe Básica

```java
// classe principal
public class HelloWorld {
  public static void main(String[] args) {
    System.out.println("Olá mundo!");
  }
}

// variáveis e tipos
int idade = 25;         // Inteiro
double preco = 19.99;   // Número decimal
String nome = "João";   // Texto
boolean ativo = true;   // Booleano
char letra = 'A';       // Caracterer único
```

## Operadores

```java

// Aritméticos
int a = 10, b = 3;
int soma      = a + b;  // 13
int diferenca = a - b;  // 7
int produto   = a * b;  // 30
int divisao   = a / b;  // 3
int resto     = a % b;  // 1

// Comparação
boolean igual     = (a == b); // false
boolean diferente = (a != b); // true
boolean maior     = (a > b);  // true
menorIgual        = (a <= b); // false

// Lógicos
boolean resultadoAND = (a > 5) && (b < 5);  // true (E)
boolean resultadoOR  = (a > 5) || (b > 5);  // true (OU)
boolean resultadoNOT = !(a == b);           // true (NÃO)
```

## Estruturas de Controle

```java
// if-else
if (idade >= 18) {
  System.out.println("Maior de idade");
} else if (idade >= 13) {
  System.out.println("Adolescente");
} else {
  System.out.println("Criança");
}

// Switch
int dia = 3;
switch (dia) {
  case 1:
    System.out.println("Segunda");
    break;

  case 2:
    System.out.println("Terça");
    break;

  default:
    System.out.println("Outro dia");
}

// Ternário
String status = (idade >= 18) ? "Maior" : "Menor";
```

## Loops

```java
for (int i = 0; i < 5; i++) {
  System.out.println(i);  // 0, 1, 2, 3, 4
}

// For-each (arrays/coleções)
String[] cores = {"Vermelho", "Verde", "Azul"};
for (String cor : cores) {
  System.out.println(cor);
}

// While
int contador = 0;
while (contador < 3) {
  System.out.println(contador);
  contador++;
}

// Do-while (executa pelo menos uma vez)
int x = 0;
do {
  System.out.println(x);
  x++;
} while (x < 3);
```

## Arrays e Coleções

```java
// Array (tamanho fixo)
int[] numeros = {1, 2, 3, 4, 5};
System.out.println(numeros[0]);     // 1
numeros[2] = 10;

// Array 2D
int[][] matriz = {{1, 2}, {3, 4}};
System.out.println(matriz[0][1]);   // 2

// ArrayList (tamanho dinâmico)
ArrayList<String> lista = new ArrayList<>();
lista.add("Java");
lista.add("Python");
lista.remove(0);
System.out.println(lista.size());   // 1

// HashMap (chave-valor)
HashMap<String, Integer> mapa = new HashMap<>();
mapa.put("João", 25);
mapa.put("Maria", 30);
System.out.println(mapa.get("João")); // 25
```

## Métodos

```java
// Método sem retorno
public void saudar(String nome) {
  System.out.println("Olá, " + nome);
}

// Método com retorno
public int somar(int a, int b) {
  return a + b;
}

// Método com múltiplos parâmetros
public double calcularMedia(double... notas) {
  double = soma = 0;
  for (double nota : notas) {
    soma += nota;
  }
  return soma / notas.length;
}

// Chamando Métodos
saudar("Gabriel");                      // Olá, Gabriel
int resultado = somar(5, 3);            // 8
double media = calcularMedia(7, 8, 9);  // 8.0
```

## Orientação a Objetos

```java
// Classe
public class Pessoa {
  // Atributos
  private String nome;
  private int idade;

  // Constructor
  public Pessoa(String nome, int idade) {
    this.nome = nome;
    this.idade = idade;
  }

  // Getters e Setters
  public String getNome() {
    return nome;
  }

  public void SetNome(String nome) {
    this.nome = nome;
  }

  // Método
  public void apresentar() {
    System.out.println("Sou " + nome + " e tenho " + idade + " anos.");
  }
}

// Usando a classe
Pessoa p = new Pessoa("Gabriel", 35);
p.apresentar();                       // Sou Gabriel e tenho 35 anos
p.setNome("Gabriel Leite");
p.getName();                          // Gabriel Leite
```

## Herança e Polimorfismo

```java
// Classe pai
public class Animal {
  public void fazerSom() {
    System.out.println("Som genérico");
  }
}

// Classe filha
public class Cachorro extends Animal {
  @Override
  public void fazerSom() {
    System.out.println("Au au!");
  }
}

// Uso
Animal cusco = new Cachorro();
cusco.fazerSom();              // Au au!
```

## Tratamento de exceções

```java
try {
  int resultado = 10 / 0; // Erro!
} catch (ArithmeticException e) {
  System.out.println("Erro: " + e.getMessage());
} catch (Exception e) {
  System.out.println("Erro geral.");
} finally {
  System.out.println("Sempre executado");
}
```

## Strings

```java
String texto = "Java";

// Métodos úteis
System.out.println(texto.length());           // 4
System.out.println(texto.toUpperCase());      // JAVA
System.out.println(texto.toLowerCase());      // java
System.out.println(texto.charAt(0));          // J
System.out.println(texto.substring(1));       // ava
System.out.println(texto.contains("va"));     // true
System.out.println(texto.replace("a", "o"));  // Jovo

// Concatenação
String saudacao = "Olá, " + "Mundo!";
String interpolacao = String.format("Valor: %d", 42);
```

## Dicas Importantes

- **import**: Adicione imports no início do arquivo (ex: `import java.util.*;`).
- **main**: Toda aplicação precisa de um método `main` para executar.
- **null**: Sempre verifique se um objeto não é nulo antes de usar.
- **Convenção**: Use camelCase para variáveis/métodos, PascalCase para classe.
- **Comentários**: Use `//` para uma linha, `/* */` para múltiplas linhas.
