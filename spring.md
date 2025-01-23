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
