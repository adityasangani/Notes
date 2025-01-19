# Rest API
We send data from server to client in the form of XML/JSON format. REST Api are just a bunch of rules and conventions that people must follow when building and interacting with web services.
So initially we have classes and objects. We must somehow convert them into XML/JSON. 
- When you send a request for myresource, the request goes to web.xml file, and this sends a request to ServletContainer, which then goes through MyResource.java file, inside which we have @GET request.
