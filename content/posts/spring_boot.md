+++
date = '2026-03-21T22:18:24-03:00'
draft = false
title = 'Spring Boot'
+++

# Spring Boot - Guia Prático

## O que é Spring Boot?

**Spring Boot** é um framework que simplifica drasticamente o desenvolvimento de aplicações Java. Ele automatiza configurações complexas do Spring Framework tradicional, permitindo que você crie aplicaçẽos robustas com pouco código.

> "É como a diferença entre configurar um servidor Node.js manualmente versus usar Express. **Spring Boot** é o `Express.js` do Java".

## Por que usar Spring Boot?

| **Problema**              | **Solução Spring Boot**                      |
| ------------------------- | -------------------------------------------- |
| Configuração XML complexa | Configuração automática inteligente          |
| Dependências conflitantes | Gerencia versões compatíveis automaticamente |
| Deploy complicado         | Cria JAR executável com servidor embutido    |
| Falta de padrões          | Força arquitetura MVC/REST bem definida      |
| Muita escrita de código   | Reduz código boilerplate significativamente  |

## Criando projeto Spring Boot

### Via Spring Initializr

Acesse [Spring Initializr](https://start.spring.io) baixe e descompacte o projeto já configurado.

### Via Maven

```sh
mvn archetype:generate \
  -DgroupId=com.empresa \
  -DartifactId=meu-app \
  -DarchetypeArtifactId=maven-archetype-quickstart
```

## Executando a aplicação

```sh
# Executar via Maven
mvn spring-boot:run

# compliar e executar JAR
mvn clean package
java -jar target/meu-app-1.0.0.jar
```

## Exemplo de aquito `pom.xml` básico

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.empresa</groupId>
    <artifactId>meu-app</artifactId>
    <version>1.0.0</version>

    <!-- Parent do Spring Boot (gerencia versões) -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.2.0</version>
        <relativePath/>
    </parent>

    <properties>
        <java.version>11</java.version>
    </properties>

    <dependencies>
        <!-- Web (REST APIs) -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!-- Banco de dados -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.33</version>
        </dependency>

        <!-- Testes -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>

```

## Estrutura de um Projeto Spring Boot

```sh
src/
├── main/
│   ├── java/com/empresa/app/
│   │   ├── Application.java          # Classe principal
│   │   ├── controller/
│   │   │   └── ProductController.java
│   │   ├── service/
│   │   │   └── ProductService.java
│   │   ├── repository/
│   │   │   └── ProductRepository.java
│   │   └── model/
│   │       └── Product.java
│   └── resources/
│       ├── application.properties    # Configurações
│       ├── application-dev.properties
│       └── application-prod.properties
└── test/
    └── java/com/empresa/app/
        └── ProductControllerTest.java

```
