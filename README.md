# Introdu��o JPA e Hibernate (b�nus Maven e MySQL) - Aul�o #006
###### DevSuperior - sua carreira dev com fundamento de ensino superior


Assista o v�deo desta aula:

[![Image](https://img.youtube.com/vi/CAP1IPgeJkw/mqdefault.jpg "V�deo no Youtube")](https://youtu.be/CAP1IPgeJkw)

## Sum�rio
- [O que voc� vai aprender]
- [Pr�-requisitos]
- [Vis�o geral sobre mapeamento objeto-relacional]
- [JPA]
- [Criando uma aplica��o simples]

## O que voc� vai aprender
- Vis�o geral sobre mapeamento objeto-relacional
- Introdu��o ao JPA - Java Persistence API

## Pr�-requisitos

- L�gica de programa��o
- OO b�sica
- BD b�sico

## Vis�o geral sobre mapeamento objeto-relacional

![myImage](https://github.com/devsuperior/aulao006/raw/master/img-problema-orm.png)

### Outros problemas que devem ser tratados:
- Contexto de persist�ncia (objetos que est�o ou n�o atrelados a uma conex�o em um dado momento)
- Mapa de identidade (cache de objetos j� carregados)
- Carregamento tardio (lazy loading)
- Outros

## JPA

Java Persistence API (JPA) � a especifica��o padr�o da plataforma Java EE (pacote javax.persistence) para mapeamento objeto-relacional e persist�ncia de dados.

JPA � apenas uma especifica��o (JSR 338):
http://download.oracle.com/otn-pub/jcp/persistence-2_1-fr-eval-spec/JavaPersistence.pdf

Para trabalhar com JPA � preciso incluir no projeto uma implementa��o da API (ex: Hibernate).

Arquitetura de uma aplica��o que utiliza JPA:

![myImage](https://github.com/devsuperior/aulao006/raw/master/img-arquitetura-jpa.png)

### Principais classes:

#### EntityManager
https://docs.oracle.com/javaee/7/api/javax/persistence/EntityManager.html

Um objeto EntityManager encapsula uma conex�o com a base de dados e serve para efetuar opera��es de acesso a dados (inser��o, remo��o, dele��o, atualiza��o) em entidades (clientes, produtos, pedidos, etc.) por ele monitoradas em um mesmo contexto de persist�ncia.

Escopo: tipicamente mantem-se uma inst�ncia �nica de EntityManager para cada thread do sistema (no caso de aplica��es web, para cada requisi��o ao sistema). 

#### EntityManagerFactory
https://docs.oracle.com/javaee/7/api/javax/persistence/EntityManagerFactory.html

Um objeto EntityManagerFactory � utilizado para instanciar objetos EntityManager.

Escopo: tipicamente mantem-se uma inst�ncia �nica de EntityManagerFactory para toda aplica��o.

### Passos

#### Crie uma base de dados MySQL vazia
- No Workbrench, crie uma base de dados chamada "aulajpa"

#### Crie um novo projeto Maven
- File -> New -> Other -> Maven Project
- Create Simple Project -> Next
  - Group Id: com.educandoweb
  - Artifact Id: aulajpamaven
  -Finish

#### Atualize o Maven do projeto para Java 11
- Edite o arquivo pom.xml
- Inclua o conte�do abaixo
- Salve o projeto
- Bot�o direito no projeto -> Maven -> Update Project

```xml
<properties>
	<maven.compiler.source>11</maven.compiler.source>
	<maven.compiler.target>11</maven.compiler.target>
</properties>
```

#### Inclua as depend�ncias Maven a serem baixadas:

```xml
<dependencies>
	<!-- https://mvnrepository.com/artifact/org.hibernate/hibernate-core -->
	<dependency>
		<groupId>org.hibernate</groupId>
		<artifactId>hibernate-core</artifactId>
		<version>5.4.12.Final</version>
	</dependency>

	<!-- https://mvnrepository.com/artifact/org.hibernate/hibernate-entitymanager -->
	<dependency>
		<groupId>org.hibernate</groupId>
		<artifactId>hibernate-entitymanager</artifactId>
		<version>5.4.12.Final</version>
	</dependency>

	<!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
	<dependency>
		<groupId>mysql</groupId>
		<artifactId>mysql-connector-java</artifactId>
		<version>8.0.19</version>
	</dependency>
</dependencies>
```

#### Configure o JPA no seu projeto por meio do arquivo persistence.xml
- Crie uma pasta "META-INF" a partir da pasta "resources"
- Dentro da pasta META-INF crie um arquivo "persistence.xml"
- Conte�do do arquivo persistence.xml:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<persistence xmlns="http://xmlns.jcp.org/xml/ns/persistence"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/persistence
    http://xmlns.jcp.org/xml/ns/persistence/persistence_2_1.xsd"
	version="2.1">

	<persistence-unit name="exemplo-jpa" transaction-type="RESOURCE_LOCAL">
	<properties>
		<property name="javax.persistence.jdbc.url"
			value="jdbc:mysql://localhost/aulajpa?useSSL=false&amp;serverTimezone=UTC" />

		<property name="javax.persistence.jdbc.driver" value="com.mysql.jdbc.Driver" />
		<property name="javax.persistence.jdbc.user" value="root" />
		<property name="javax.persistence.jdbc.password" value="" />

		<property name="hibernate.hbm2ddl.auto" value="update" />

		<!-- https://docs.jboss.org/hibernate/orm/5.4/javadocs/org/hibernate/dialect/package-summary.html -->
		<property name="hibernate.dialect" 	value="org.hibernate.dialect.MySQL8Dialect" />
	</properties>
	</persistence-unit>
</persistence>
```

#### Inclua os MAPEAMENTOS na classe de dom�nio:

```java
package dominio;

import (...)

@Entity
public class Pessoa implements Serializable {
	private static final long serialVersionUID = 1L;

	@Id
	@GeneratedValue(strategy=GenerationType.IDENTITY)
	private Integer id;
	(...)
```

#### Na classe "Programa" fa�a os testes (veja v�deo-aula).
