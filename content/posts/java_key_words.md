+++
date = '2026-03-23T17:20:48-03:00'
draft = false
title = 'Java Key Words'
toc = true
+++

# Guia Prático sobre as Palavras Chave Java

## Modificadores de acesso `public` `private` `protected`

`public` `private` e `protected` são modificadores de acesso que controlam a visibilidade de classes, métodos e variáveis. Eles definem quem pode acessar esses membros.

### Sem modificador

Membros definidos sem modificadores de acesso são **visíveis na mesma classe, mesmo pacote** mas **invisíveis para subclasses ou qualquer outra parte da aplicação**.

### `public` - Acesso Público

`public` é um modificador de acesse que oferece visibilidade total a classes, métodos e variáveis. Um membro `public` **pode ser acessado de qualquer lugar do programa**.

```java
public class Pessoa {
  public String nome = "Gabriel";

  public void saudar() {
    System.out.println("Olá!");
  }
}

// Em outra classe, em outro pacote:
Pessoa p = new Pessoa();
System.out.println(p.nome); // OK. Dado acessível.
p.saudar();                 // OK. Dado acessível.
```

### `private` - Acesso Privado

`private` é um modificador de acesse que oferece visibilidade limitado a classes, métodos e variáveis. Um membro `private` **só pode ser acessado dentro dad mesma classe**.

```java
public class Conta {
  private double saldo = 1000.0;      // privado
  private String senha = "12345678";  // privado

  public void depositar(double valor) {
    saldo += valor; // OK. Dado acessível aqui.
  }
}

// Em outra classe:
Conta c = new Conta();
// System.out.println(c.saldo); // Erro. Tentativa de acessar dado privado.
// c.senha = "99999";           // Erro. Tentativa de modificar dado privado.
```

#### Por que usar `private`? - Encapsulamento (proteção de dados)

No exemplo à seguir o membro `saldo` é protegido. Você não pode alterar diretamente, mas pode usar métodos que fazem validações.

```java
public class Conta {
  private double saldo = 1000.0;

  // Método público para acessar o saldo
  public double getSaldo() {
    return saldo;
  }

  // Método público para depositar (com validação)
  public void depositar(double valor) {
    if (valor > 0 ) {
      saldo += valor;
    }
  }
}

Conta c = new Conta();
System.out.println(c.getSaldo()); // OK. Acesso controlado
c.depositar(500);                 // OK. Acesso validado.
// c.depositar(-9999);            // Erro. Valor inválido detectado.
// c.saldo = 1000;                // Erro. Tentativa de acesso a membro privado.
```

#### Exemplo prático completo

```java
public class Carro {
  // Privado - dados sensíveis
  private String placa;
  private double velocidade = 0;

  // Público - construtor
  public Carro(String placa) {
    this.placa = placa;
  }

  // Público - métodos controlados
  public void acelerar(double aumento) {
    if (aumento > 0 && velocidade + aumento <= 200) {
      velocidade += aumento;
    }
  }

  public double getVelocidade() {
    return velocidade;
  }

  public String getPlaca() {
    return placa;
  }
}

// Uso:
Carro c = new Carro("ABC1234");
c.acelerar(50);                         // OK. Método público.
System.out.println(c.getVelocidade());  // 50
// c.velocidade = 500;                  // Erro. Tentativa de acessar dado privado.
```

#### Resumo de diferenças

| Aspecto        | `public`            | `private`               |
| -------------- | ------------------- | ----------------------- |
| **Acesso**     | De qualquer lugar   | Apenas dentro da classe |
| **Segurança**  | Baixa               | Alta                    |
| **Uso típico** | Interfáce da classe | Dados internos          |
| **Exemplo**    | Métodos úteis       | Vatiáveis sensíveis     |

> **Regra de ouro**: Use `private` para dados internos e `public` apenas para o que realmente precisa ser acessado de fora.

### `protected` - Acesso intermediário

`protected` é um modificador de acesso que oferece um nível intermediário de visibilidade entre `private` e `public`. Um membro `protected` pode ser acessado pela classe, pelo mesmo pacote e pelas subclasses (mesmo) em pacotes diferentes).

| Quem acessa                   | Acesso |
| ----------------------------- | :----: |
| Mesma classe                  |   ✅   |
| Mesmo pacote                  |   ✅   |
| Subclasse (herança)           |   ✅   |
| Outras classes fora do pacote |   ❌   |

#### Exemplo Prático

```java
public class Animal {
  protected String nome;  // Protegido
  private String dna;     // Privado
  public String especie;  // Público

  protected void fazerSom() {
    System.out.println(nome + " faz som.");
  }
}

// Subclasse em outro pacote
public class Cachorro extends Animal {
  public void latir() {
    // - [x] Pode acessar membros protected da superclasse
    Sytesm.out.println(nome + " está latindo!");
    fazerSom(); // - [x] Pode chamar método protected

    // System.out.println(dna); // Erro. Tentativa de acessar dado privado.
  }
}

// Outra classe fora do pacote
public class Teste {
  public static void main(String[] args) {
    Cachorro c = new Cachorro();
    // System.out.println(c.nome); // Erro. Tentativa de acessar dado protegido.
    // c.fazerSom(); // Erro. Tentativa de acessar dado protegido.
    System.out.println(c.especie); // - [x] OK, o dado é público.
  }
}
```

#### Quando usar `protected` ?

`protected` é ideal quando você quer:

- Permitir que as subclasses acessem dados ou métodos.
- Manter a encapsulação contra classes não-relacionadas.
- Criar uma interface para herança sem expor tudo publicamente.

> Use `protected` para membros que fazem sentido serem herdados, mas `private` para dados internos que devem ficar completamente ocultos.

**1 Método que as subclasses podem sobreescrever**

```java
public class Veiculo {
  protected void ligar() {
    System.out.println("Veiculo ligado");
  }
}

public classs Carro extends Veiculo {
  @Override
  protected void ligar() {
    System.out.println("Carro ligado com chave");
  }
}
```

**2 Dados que as subclasses precisam acessar**

```java
public class Pessoa {
  protected String cpf; // Subclasses podem acessar
  protected int idade;  // Subclasses podem acessar
  private String senha; // Apenas Pessoa pode acessar
}

public Class Funcionario extends Pessoa {
  public void exibirDados() {
    System.out.println(cpf); // OK. Acesso permitido
    System.out.println(idade); // OK. Acesso permitido
    // System.out.println(senha); // Erro. Tentativa de acessar dado privado.
  }
}
```

**3 Métodos auxiliares para subclasse**

```Java
public class FormaGeometrica {
  protected double calcularArea() {
    return 0; // implementação padrão
  }
}

public class Retangulo extends FormaGeometrica {
  private double largura, altura;

  @Override
  protected double calcularArea() {
    return largura * altura; // Subclasse pode sobreescrever
  }
}
```

### Exemplo completo Hierarquia de classes

```java
public class Animal {
  protected String nome;  // Subclasses podem acessar
  private String dna;     // Apenas Animal acessa
  public String especie;  // Qualquer um acessa

  protected void mover() {
    System.out.println(nome + " está se movendo.");
  }
}

public class Mamifero extends Animal {
  public void amamentar() {
    // - [x] Pode acessar protected
    System.out.println(nome + " está amamentando.");
    mover(); // - [x] Pode chamar método protected
  }
}

public class Cachorro extends Mamifero {
  public void latir() {
    // - [x] Herda acesso a protected de Animal
    System.out.println(nome + " está latindo.");
  }
}

// Uso:
Cachorro c = new Cachorro();
// c.nome = "Rex"; // Erro. Tentativa de acessar dado protegido.
// System.out.println(c.dna); // Erro. Tentativa de acessar dado privado.
System.out.println(c.especie); // - [x] OK. Acesso permitido. Dado público.
```

### Comparação

| Modificador       | Mesma classe | Mesmo pacote | Subclasse | Qualquer lugar |
| ----------------- | :----------: | :----------: | :-------: | :------------: |
| `private`         |      ✅      |      ❌      |    ❌     |       ❌       |
| (sem modificador) |      ✅      |      ✅      |    ❌     |       ❌       |
| `protected`       |      ✅      |      ✅      |    ✅     |       ❌       |
| `public`          |      ✅      |      ✅      |    ✅     |       ✅       |

## Static

`static` é uma palavra-chave que marca um membro (variável ou método) como **pertencente à classe em si, e não a instâncias individuais** da classe. Significa que existe apenas uma cópia desse membro, compartilhada por todas as instâncias.

### Variáveis estáticas

Uma variável `static` é compartilhada por todas as instâncias da classe:

No exemplo à seguir, `total` é incrementada a cada nova instância criada, mas existe apenas uma única dessa variável na memória.

```java
public class Contador {
  static int total = 0; // Compartilhado por todas as instâncias
  int id;

  public Contador() {
    total++;
    id = total;
  }
}

Contador c1 = new Contador();
Contador c2 = new Contador();
System.out.println(c1.id);          // 1
System.out.println(c2.id);          // 2
System.out.println(Contador.total); // 2
```

### Métodos estáticos

Um método `static` não precisa de uma instância para ser chamado:

```java
public class Calculadora {
  static int somar(int a, int b) {
    return a + b;
  }
}

int resultado = Calculadora.somar(5, 3);  // chamado diretamente na classe
System.out.println(resultado);            // 8
```

Não há necessidade de criar um objeto `new Calculadora()` para usar o método.

| Aspecto          | Estático                        | Não-estático                       |
| ---------------- | ------------------------------- | ---------------------------------- |
| Acesso           | Pela classe (`Classe.metodo()`) | Por instância( `objeto.metodo()`)  |
| Cópia em memória | Uma única cópia                 | Uma cópia por instância            |
| Acesso a `this`  | Não tem acesso                  | Tem acesso                         |
| Uso típico       | Utilitários, constantes         | Comportamento específico do objeto |

### Exemplos práticos

**Constantes estáticas**

```java
public class Configuracao {
  static final double PI = 3.14159;
  static final string VERSAO = "1.0";
}

System.out.println(Configuracao.PI);  // 3.14159
```

**Método utilitário**

```java
public class StringUtils {
  static String reverter(String texto) {
    retur new StringBuilder(texto).reverse().toString();
  }
}

String invertido = StringUtils.reverter("Gabriel"); // "leirbaG"
```

### ⚠️ Restrições importantes

Um método `static` não pode acessar membros não-estáticos diretamente. Isso ocorre porque métodos estáticos não tem acesso a `this` (não está vinculado a uma instância específica).

```java
public class Exemplo {
  int valor = 10; // não-estático

  static void exibir() {
    // System.out.println(valor); // Erro. Método estático tentando acessar dado não estático.
    System.out.println("Olá");    // OK.
  }
}
```

## Final

`final` é uma palavra-chave que marca algo como **constante, imutável ou não-sobrescritível**, dependendo do contexto. Uma vez definido, não pode ser alterado ou estendido.

### Variáveis finais

Uma variável `final` não pode ter seu valor alterado após a atribuição inicial.

```java
final int idade = 25;
// idade = 30; // Erro de compilação.

final String nome = "Gabriel";
// nome = "Gabriel Leite"; // Erro de compilação.
```

A variável deve ser inicializada **obrigatoriamente** (na declaração ou no contrutor);

```java
public class Pessoa {
  final string cpf;

  public Pessoa(String cpf) {
    this.cpf = cpf; // Inicialização no construtor
  }
}
```

### Métodos finais

Um método `final` não pode ser sobreescrito por subclasses:

```java
public class Animal {
  final void fazerSom() {
    System.out.println("Som do animal.");
  }
}

public class Cachorro extends Animal {
  // void fazerSom() {} // Erro. Não é possível reescrever um método final
}
```

> Isso garante que a implementação do método não seja alterado pelas subclasses.

### Classes finais

Uma classe `final` não pode ser estendida:

```java
final public class Imovel {
  // ...
}

// public class Casa extends Imovel {} // Erro. Não é possível extender uma classe final
```

Exemplos reais: `String`, `Integer`, `Double` são classes finais do Java.

### Comparação `final` em diferentes contextos

| Contexto | Efeito                                   |
| -------- | ---------------------------------------- |
| Variável | Não pode ter seu valor alterado          |
| Método   | Não pode ser sobreescrito por subclasses |
| Classe   | Não pode ser estendida                   |

### Quando usar `final`

- **Constantes**: valores que nunca devem mudar
- **Segurança**: impedir que métodos críticos sejam alterados
- **Performance**: o compilador pode otimizar código `final`
- **Imutabilidade**: criar objetos que não podem ser modificados

### Exemplos práticos

#### Constantes

```java
public class Configuracao {
  static final double PI = 3.14159;
  static final String VERSAO = "2.0";
}
```

#### Parâmetros finais

```java
void calcular(final int valor) {
  // valor = 10; // Erro.
  System.out.println(valor);
}
```

#### Classe imutável

```java
final public class Ponto {
  final double x;
  final double y;

  public Ponto(double x, double y) {
    this.x = x;
    this.y = y;
  }
}
```

## Record

Em **Java**, `record` é uma forma consisa de declarar uma classe que funciona principalmente como um **contentor de dados imutáveis**. Introduzido no Java 16 (como recurso final, parte da linguagem oficial), o `record` reduz significativamente a quantidade de código boilerplate necessário.

### Sintaxe

```java
public record Pessoa(String nome, int idade) {}
```

Isso é equivalente a uma classe tradicional com:

- Campos privados e finais(nome e idade)
- Contrutor canônico que aceita todos os parâmetros
- Métodos `getter` automáticos (`nome()` e `idade()`)
- Métodos `equals()`, `hashCode` e `toString()` gerados automaticamente

### Características Principais

**Imutabilidade**

Os campos de um `record` são sempre **finais**, ou seja não podem ser alterados após a criação.

```java
Pessoa p = new Pessoa("João", 30);
p.nome(); // retorna "João"
// p.nome = "Maria"; // Erro de compilação
```

**Uso prático**

```java
record Ponto(double x, double y) {}

Ponto p1 = new Ponto(10.5, 20.3);
Ponto p2 = new Ponto(10.5, 20.3);

System.out.println(p1.equals(p2));  // true
System.out.println(p1);           // Ponto[x=10.5, y=20.3]
```

#### Quando Usar `record`

- Transferência de dados entre métodos ou classes
- Dados imutáveis (que não precisam mudar)
- Reduzir código em comparação com classes tradicionais
- Estruturas simples sem lógica complexa

> **Não use `record` quando precisar de campos mutáveis, herança ou lógica complexa de classe.**

#### Exemplos de como seria o código _com_ e _sem_ `record`

```java
// Com record

public record Produto (String nome, double preco) {}

// Uso:
List<Produto> produtos = Arrays.asList(
  new Produto("Notebook", 2500.0),
  new Produto("Mouse", 50.0)
);

// Funciona perfeitamente com Set, HashMap, etc.
Set<Produto> unicos = new HashSet<>(produtos);
```

```java
// Sem record

public class Produto {
  private final String nome;
  private final double preco;

  public Produto(String nome, double, preco) {
    this.nome = nome;
    this.preco = preco;
  }

  public String nome() {
    return nome;
  }

  public String preco() {
    return preco;
  }

  @Override
  public boolean equals(Object o) {
    if (this == 0) return true;
    if (o == null || getClass() != o.getClass()) return false;
    Produto produto = (Produto) o;
    return Double.compare(produto.preco, preco) == 0 && Objects.equals(nome, produto.nome);
  }

  @Override
  public int hashCode() {
    return Object.hash(nome, preco);
  }

  @Override
  public String toString() {
    return "Produto{" + "nome=" + nome + ", preco=" + preco + '}';
  }
}

// Uso:
List<Produto> produtos = Arrays.asList(
  new Produto("Notebook", 2500.0),
  new Produto("Mouse", 50.0)
);

Set<Produto> unicos = new HashSet<>(produtos);

```
