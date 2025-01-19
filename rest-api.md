# REST API
We send data from server to client in the form of XML/JSON format. REST Api are just a bunch of rules and conventions that people must follow when building and interacting with web services.
So initially we have classes and objects. We must somehow convert them into XML/JSON. 
- When you send a request for myresource, the request goes to web.xml file, and this sends a request to ServletContainer, which then goes through MyResource.java file, inside which we have @GET request.

## REST API in Spring Boot
![image](https://github.com/user-attachments/assets/94db485e-694d-46bd-857f-d453c7638310)

To connect the project to a mysql database, go to application.properties file and write: 
```
  spring.datasource.url=jdbc:mysql://localhost:3306/restdb
  spring.datasource.username=root
  spring.datasource.password=0
  spring.datasource.driver-class-name=com.mysql.jdbc.Driver
```

### Get Request
Instead of @RequestMapping we write @GetMapping("/hello")
