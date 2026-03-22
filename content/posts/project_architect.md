+++
title = 'Organização de Projeto Java e Ferramentas de Build'
date = 2026-03-21T19:56:52-03:00
draft = false
+++

# Estrutura de Projeto Java

A estrutura mais comum em projetos Java segue o padrão **Maven**, que é amplamente adotado no mercado. Aqui está a organização típica:

```
meu-projeto/
├── src/
│   ├── main/
│   │    ├── java/
│   │    │   └── com/
│   │    │       └── empresa/
│   │    │           └── app/
│   │    │               ├── Main.java
│   │    │               ├── controller/
│   │    │               ├── service/
│   │    │               ├── repository/
│   │    │               └── model/
│   │    └── resources/
│   │        ├── application-properties
│   │        └── logback.xml
│   └── test/
│       ├── java/
│       │   └── com/empresa/app/
│       │       └── AppTeste.java
│       └── resources/
├── pom.xml (ou build.gradle)
├── README.md
└── .gitignore
```

## Maven - O Padrão do Mercado

**Maven** gerencia dependências, compila código, executa testes e empacota a aplicação. **Maven** está para **Java** assim como **npm** está para **javascript**.

### Comandos Úteis

```sh
# Verificar Instalação
mvn --version

# Criar novo projeto
mvn archetype:generate \
  -DgroupId=com.empresa \
  -DartifactId=meu-app \
  -DarchetypeArtifactId=maven-archetype-quickstart

# Instalar dependências
mvn install

# Compilar código
mvn compile

# Executar testes
mvn test

# Empacotar em JAR/WAR
mvn package

# Limpar arquivos geardos
mvn clean

# Executar aplicação (com Spring Boot)
mvn spring-boot:run

# Atualizar dependências
mvn dependency:resolve
```

### Exemplo de arquivo de configuração gerado para o projeto `pom.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project>
    <modelVersion>4.0.0</modelVersion>

    <!-- Identificação do projeto -->
    <groupId>com.empresa</groupId>
    <artifactId>meu-app</artifactId>
    <version>1.0.0</version>
    <name>Minha Aplicação</name>

    <!-- Propriedades -->
    <properties>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <!-- Dependências -->
    <dependencies>
        <!-- JUnit para testes -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.2</version>
            <scope>test</scope>
        </dependency>

        <!-- Spring Framework -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <version>3.0.0</version>
        </dependency>

        <!-- Banco de dados -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.33</version>
        </dependency>
    </dependencies>

    <!-- Plugins de build -->
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.10.1</version>
            </plugin>
        </plugins>
    </build>
</project>
```

## Gradle - Altenativa moderna

**Gradle** é mais moderno que **Maven** e está crescendo em adoção, especialmente me projetos novos.

### Comandos úteis

```sh
# Instalar dependências
gradle build

# Compilar
gradle compile

# Executar testes
gradle test

# Executar aplicação
gradle run

# Limpar
gradle clean
```

### Arquivo de configuração `build.gradle`

```gradle
plugins {
    id 'java'
    id 'application'
}

group = 'com.empresa'
version = '1.0.0'
sourceCompatibility = '11'

repositories {
    mavenCentral()
}

dependencies {
    // Testes
    testImplementation 'junit:junit:4.13.2'

    // Spring Boot
    implementation 'org.springframework.boot:spring-boot-starter-web:3.0.0'

    // Banco de dados
    implementation 'mysql:mysql-connector-java:8.0.33'
}

tasks.named('test') {
    useJUnitPlatform()
}
```

## Comparativo entre as Ferramentas

| Ferramentas | Descrição                                                                                | Uso no Mercado                                                     | Arquivo Config |
| ----------- | ---------------------------------------------------------------------------------------- | ------------------------------------------------------------------ | -------------- |
| **Maven**   | Gerenciador de dependências e build mais tradicional. Usa `XML`. Muito maduro e estável. | **Extremamente utilizado** em empresas grandes e projetos legados. | `pom.xml`      |
| **Gradle**  | Mais moderno que Maven, usa `Groovy/Kotlin`. Mais rápido e flexível.                     | **Crescente adoção**, especialmente em Android e projetos novos.   | `build.gradle` |
| **Ant**     | Mais antigo, menos usado atualmente. Requer mais configuração manual.                    | Projetos legados apenas.                                           | `build.xml`    |

## Dicas Práticas

- **Para novos projetos**: Use **Gradle** ou **Spring Boot** pela facilidade.
- **Para empresas**: Provavelmente usarão **Maven** pois é o padrão consolidado.
- **IDE** recomendada: **IntelliJ IDEA** ou **Eclipse** já que gerenciam **Maven/Gradle** automaticamente.
- **Dependências online**: Procure em [mvnrepository.com](https://mvnrepository.com)
- **Versionamento**: Use `MAJOR.MINOR.PATCH` (ex: 1.2.3)
