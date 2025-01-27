# Spring Boot
- Spring Boot is a framework for building applications in the Java programming language.
- We can create Spring applications using Spring Boot.
- Spring Boot came in order to easily configure and run Spring applications.
- Spring Boot has an already embedded Tomcat server.

- We use the @SpringBootApplication annotation on the main class. This single annotation replaces the need for setting up a manual Spring application context.
- We no longer need to explicitly create an application context using AnnotationConfigApplicationContext as Spring Boot handles that behind the scenes.
- We use SpringApplication.run() to start the application, and Spring Boot takes care of configuring the embedded web server and other necessary components.

## Jar Vs War
Jar = Java Archive.
War = Web Application Archive.
You can run Jar using commands. But for War, you will have to deploy on external web server.

## Maven Lifecycle
1. validate - validate the project is correct and all necessary information is available.
2. compile - compile the source code of the project.
3. test - test the compiled source code using a suitable unit testing framework. These tests should not require the code be packaged or deployed.
4. package - take the compiled code and package it in its distributable format such as JAR.
5. verify - run any checks on results of integration tests to ensure quality criteria are met.
6. install - install the package into the local repository, for use as a dependency in other projects locally.
7. deploy - done in the build environment, copies the final package to the remote repository for 

We can run spring boot like this also: 
```
mvn package
//then go to target
cd target
//then do ls
ls
//find the packaged jar file. this is a fat jar file which has compiled code + all the dependencies
java -jar <nameofpackagedfile>
```

mvn install - humare local m2 folder mein jar install karke dega.
mvn clean - target se saari cheezein udd jayegi