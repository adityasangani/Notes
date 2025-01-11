## JDBC (Java Database Connectivity)
We need to create a GUI in which if a person clicks a button, his data would be stored in the database (general usecase). This requires java code (if we are doing web dev in java). 
- So JDBC helps connect database with java code.
- There are 4 types of drivers that we can use for this purpose.
  There are 7 steps to perform this connectivity.
  1. import the package (GET A PHONE) - java.sql.*
  2. a. load the driver - mysql, postgres, oracle; all have different drivers. for mysql it is com.mysql.jdbc.driver
     b. register the driver (GET A NETWORK SO THAT YOU CAN CALL, BASICALLY SIM CARD) - class.forName("com.mysql.jdbc.driver");
  3. establish the connection (DIAL NUMBER AND CALL) - instantiate an interface called Connection - DriverManager.getConnection("URL", "username", "password")
  4. create the statement (THINK WHATEVER YOU ARE GOING TO SAY) - normal statement, prepared statement, callable statement. Statement st = con.createStatement();
  5. execute the query (SAY IT) - ResultSet rs = st.executeQuery("select * from student");
  6. process results (GET BACK RESPONSE)
  7. close the connection and statement (CLOSE)
After executing the query, initially the pointer will be here:
![image](https://github.com/user-attachments/assets/40eb53b9-c67e-49bf-be68-1b5be39dec58)
To shift the pointer down, do - rs.next(); It will shift the pointer to 1.
Now if we do, rs.getInt(<columnNumber>) gives us 1. rs.getString(2) will give us Navin. 
We can also, instead do:
```
while(rs.next()){
  System.out.println(rs.getInt(1) + " " + rs.getString(2));
} 
st.close();
con.close();
```

![image](https://github.com/user-attachments/assets/575a28ff-0ffd-4175-99f8-50dc7076483c)

If we want to insert a row into our table, 
int count = st.executeUpdate(query); (executeUpdate will return the number of rows that were affected)

First start your MySQL database.
Then, in Intellij,
- String url = "jdbc:mysql://localhost:3306/new_practice"; here new_practice is the name of the database.
- String username = "root";
- String password = whatever the password is
- Then in try catch block, create a connection: Connection con = DriverManager.getConnection(url, username, password);
- Statement st = con.createStatement();
- Now, we will execute this query. Upon executing, we will get it in tabular form which we will store in a datatype called ResultSet.
- ResultSet rs = st.executeQuery("select * from student");
- Now to print it, we will first bring the pointer to the first row, by doing rs.next();
- Then, Integer id = rs.getInt("st_id");

ResultSet and executeQuery works only for DQL. For DML, for example when we want to insert values, we must use executeUpdate which will return the number of rows affected (int type). 
Then it becomes, String query = "insert into student values (" + userid + ", " + username + "')";
A better way is to use PreparedStatements. Then: 
- String query = "insert into student values (?,?)";
- PreparedStatement st = con.prepareStatement(query);
- st.setInt(1, userid); [for first question mark]
- st.setString(2, username); [for second question mark]
- int count = st.executeUpdate();

We use Class.forName() to register drivers.

### Class.forName()
Lets say we have a class and in it we have a static block. If we want to execute the static block without creating an object of that class, we can do so by loading the class using: Class.forName("<name of the class>");
Class.forName("com.mysql.jdbc.Driver"); is the same as DriverManager.registerDriver(new com.mysql.jdbc.Driver());

### JSP & Servlets 
Normally, a client requests something from the server which the server already has, and the server responds with that object. But sometimes, a client requests something from the server which is then built in runtime dynamically. For this, the server forwards this request to an intermediary called Helper Application (Web Container). In this web container we have servlets. The servlets are basically java files which can take in the request, process it and output an html file. 
Examples of web containers are:
- Apache Tomcat
- GlassFish
- IBM WebSphere

Now how do we decide for which type of request which servlet it should be forwarded to? Well that information is written in a file called the Deployment Descriptor whose filename is web.xml file. In this web.xml file (which is basically a servlet mappping file), we mention url names; that for this type of url, use this servlet, and so on. 

Servlet is a normal java class which extends http servlet.
Now in the latest update, we don't have to use web.xml files. Instead we can use Annotations on our servlets.

Apache is the server, and Tomcat is the container. Containers are friends of webservers, and they try to do tasks which the web server cannot handle, like security, database management, life-cycle management.
Servlets are discrete java classes that reside inside the container.
![image](https://github.com/user-attachments/assets/12066e57-a7b5-41bb-9261-9957a18990c0)

- All the classes and interface required to work with Servlet API are in javax.servlet package and javax.servlet.http package.
- If you want to make a Servlet class; your class needs to implement Servlet interface DIRECTLY or INDIRECTLY.
- Servlet can be made in 2 ways: extend javax.servlet.GenericServlet, and extend javax.servlet.http.HttpServlet.
- javax.servlet.Servlet contains 3 basic lifecycle methods: init(), service(), destroy().
![image](https://github.com/user-attachments/assets/ac5ce5f2-5b58-44e5-8043-5b5c4e0156dc)

For the "GET" method, the servlet takes the values from the url. This isn't secure because what if we have some functionality using usernames and passwords. Hence to make it secure, we have to use "POST". 
When we use POST, it will store your data in cookies, and then use those values to perform the functionalities.
- GET is more faster, but POST is more secure.

- If we extend HttpServlet, we have 2 types of service: doPost and doGet. If in client we are doing get, use doGet. Similarly do the same for doPost.
- If you want to give freedom to client to do get and post both, we can either write the same function twice, once with doGet, and once with doPost; or create another method, write the logic in that, and then call the methods in the doGet and doPost methods.
![image](https://github.com/user-attachments/assets/790f45d0-3acb-4e47-84de-15494256f676)
![image](https://github.com/user-attachments/assets/82e833f2-3517-4941-baa9-e514aa8d8fb2)
![image](https://github.com/user-attachments/assets/caf393b7-fab1-4097-8941-a02f065a35ca)
![image](https://github.com/user-attachments/assets/709e6d5e-fb8d-4a3f-bc04-27184618cc96)
### Servlet Hierarchy
![image](https://github.com/user-attachments/assets/c353272d-9d74-437f-a155-9c6bb905a505)

### Counting Website Visits
![image](https://github.com/user-attachments/assets/13f721a7-6427-473e-8ea8-5f25bb2e7744)
Here, since the servlet is already inside the container, it will not call the init function again, only once. After that it will always call the service method only.

### Calling a Servlet from a Servlet
First create the two servlets, FirstServlet and SecondServlet. Now to call SecondServlet in FirstServlet, use the interface RequestDispatcher, which has two methods: forward and include.
RequestDispatcher rd = request.getRequestDispatcher("SecondServlet");
rd.forward(req, res); [Here, we are sending the same req object of FirstServlet to SecondServlet]

Now, we can also instead send the client to the second servlet directly. "Tum unse direct hi pooch lo jo tum mujhe pooch rahe the. In the previous one, client mujhe pooch raha tha, maine wo cheez second servlet ko poochi, usne mujhe jawaab diya, aur maine tumhe wo jawab bataya."
This is called "redirect":
res.sendRedirect("SecondServlet");
However, here in redirect, the SecondServlet won't have the same req object as the FirstServlet since we aren't passing the req object to SecondServlet. So in order to tackle this, we can use HttpSession.

### How to use HttpSession Session Management
- First get the data from the req object: 
String str = req.getParameter("t1");
- Create a new object of HttpSession interface like this: 
HttpSession session = request.getSession();
- Set this data in the session:
session.setAttribute("t1", str); [t1 here is just a label, and str is the data which is going to be mapped to the t1 key]

Now in the SecondServlet we must fetch this data:
- HttpSession session = req.getSession();
- String str = session.getAttribute("t1").toString(); [getAttribute will give us object. We must convert that into String]
