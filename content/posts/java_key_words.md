+++
date = '2026-03-23T17:20:48-03:00'
draft = false
title = 'Java Key Words'
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
// c.saldo = -9999;               // Erro. Valor inválido detectado.
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
