# Spring
In vscode, Ctrl+Shift+P > Spring Initializr > war
Then in App.java, starter code should look like this:
```
package com.aditya;

import org.springframework.boot.SpringApplication;

@SpringBootApplication
@ComponentScan(basePackages = {"com.aditya.demo"})
public class App 
{
    public static void main( String[] args )
    {
        SpringApplication.run(App.class, args);
        
    }
}

```
Then to run the code, use this command in vs code terminal:
```
  mvn srping-boot:run
```
### What is a Bean in Spring?
In Spring, a bean is an object that is managed by the Spring Inversion of Control (IoC) container. It is essentially a component that Spring creates, initializes, and manages throughout its lifecycle.

### Key Characteristics of a Bean:
Managed by Spring:

Spring controls the lifecycle of the object (creation, dependency injection, destruction, etc.).
Dependency Injection (DI):

Beans can be injected into other beans, reducing tight coupling and making the application more modular and testable.
Defined Through:

Annotations (e.g., @Component, @Service, @Repository, @Bean).
XML Configuration (older approach).

### JPA (Java Persistence API)
It is a set of rules to achieve ORM. Includes interfaces and annotations that you can use in your java classes to implement ORM.
To use JPA, you need a persistence provider. A persistence provider is a specific implementation of the JPA specification. Examples of JPA persistence providers include Hibernate. These providers implement the JPA interfaces and provide the underlying functionality to interact with databases.
- Spring Data JPA is built on top of the JPA specification, but it is not a JPA implementation itself. Instead, it simplifies working with JPA by providing higher level abstractions and utilities. However, to use Spring Data JPA effectively, you still need JPA implementation, such as Hibernate. 


## Maven
It is a build management tool, used for defining how your .java files get compiled to .class and then packaged into .jar/.war/.ear files.

__Steps for building software project:__
1. Downloading dependencies
2. Putting additional jars on a classpath, compiling source code into binary code.
3. Running tests.
4. Packaging compiled code into deployable artifacts such as JAR, WAR, and ZIP files.
5. Deploying these artifacts to an application server or repo.
Apache Maven automates these steps.  

Maven uses a set of identifiers:
1. groupId: a unique base name of the company or group that created the project
2. artifactId: a unique name of the project
3. version: a version of the project
4. packaging: a packaging method (eg. WAR/JAR/ZIP)

When we build, jar files are created, which go to the target folder. 

### pom.xml (Project Object Model)
The configuration of a Maven project is done via POM. POM describes the project, manages dependencies, and configures plugins for building the software. 

## Spring
It is the framework that help the developers to work on their application rather than worrying about non-functional code. So the developer can focus on business logic rather than non-functional requirement. 

Basically, Spring is a framework for dependency-injection (it means outsourcing the task of object creation) which is a pattern that allows to build very decoupled systems. 

For using spring in Eclipse:
1. Create maven project without archetype
2. In pom.xml, include following dependencies:
```
	<dependency>
	    <groupId>org.springframework</groupId>
	    <artifactId>spring-core</artifactId>
	    <version>5.3.20</version>
	</dependency>
	<!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
	<dependency>
	    <groupId>org.springframework</groupId>
	    <artifactId>spring-context</artifactId>
	    <version>5.3.20</version>
	</dependency>
```
3. Create a package in src/main/java
4. Create a class in it

### Why do we need Spring?
