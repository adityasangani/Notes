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

