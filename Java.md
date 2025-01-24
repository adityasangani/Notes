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

### Another Alternative is Cookie for Session Management
![image](https://github.com/user-attachments/assets/0467e757-184a-440c-b4ff-573cfcde5670)
Server gives client a cookie to keep for future reference. 
Now when the client is asked to take out all of its cookies and show, the client will show all the cookies to the server. 
So now to get all the cookies from the client (in the SecondServlet): 
- Cookie cookies[] = request.getCookies();
- Now, we will traverse through all the cookies until we find the Cookie with the tag "t1".

### URL Rewriting for Session Management
Now lets say we want to get the String str of FirstServlet into the SecondServlet, but this time we want to get it from the URL.
So now, in FirstServlet:
![image](https://github.com/user-attachments/assets/e22e8c76-3968-4587-bc2e-40f7c2e328eb)
And in SecondServlet:
![image](https://github.com/user-attachments/assets/0d984f43-1712-4cba-8f43-2bc73e813a6f)

### ServletConfig and ServletContext
![image](https://github.com/user-attachments/assets/1ba6faf7-888a-437c-a2bd-3bebeb69fe6f)
Use Cases
ServletConfig:
Used to initialize servlet-specific configurations like database connections or API keys.
Example: A servlet that connects to a unique database.

ServletContext:
Used for application-wide configurations, like setting global attributes or reading shared files.
Example: A shared counter for tracking the number of active users in the application.

Let's use ServletContext first:
In web.xml file:
![image](https://github.com/user-attachments/assets/fa6e3af4-11b0-4bb9-9ff0-d43b558b6bbd)
And now in FirstServlet:
![image](https://github.com/user-attachments/assets/324b1e15-addd-4b98-bc7a-9711871667ed)

Let's now use ServletConfig:
In web.xml:
![image](https://github.com/user-attachments/assets/d9cec9fa-c6bc-41c8-8742-b91c65b02334)
And now in FirstServlet:
![image](https://github.com/user-attachments/assets/e678eb73-d489-457b-89c3-fc3b00fd6977)

### File Upload in Java Servlet
First, set up your project, and download Apache Commons File Upload, and put that dependency in your pom.xml file. We will use this when we want to retrieve File from request object in servlet.
Lets say we have a form in which we take in multiple files:
![image](https://github.com/user-attachments/assets/7ef4a1a6-6581-450f-b3a2-e655780b549b)
Now since the client is sending multiple files, lets say we have a servlet called FileUpload which which has a request object which has those multiple files:
First create an object of ServletFileUpload:
![image](https://github.com/user-attachments/assets/8e13cf28-17d5-4f67-8527-00cf92a6fb76)

### JSP and How it is translated into Servlets
JSP is like React and EJS. It embeds Servlet logic into HTML. 
- Thus it makes it easier for developers to write JSP code directly, instead of creating an entire separate Servlet.
- The JSP file is internally converted into a Servlet file later on anyways btw.
- In servlets we always give a class name and then extend it right? So here, in JSP lets say the name of the file is demo.jsp. When the JSP file is converted into Servlet, the class' name will be demojsp automatically.
- JSP automatically provides several implicit objects, including request, response, session, out, etc.
  ![image](https://github.com/user-attachments/assets/8d473647-64f4-4900-9254-f2df6a8a53ba)
In JSP, the "<% ..... %>" tag is called "Scriptlet". Whatever logic we write inside the Scriptlet will be included in the service method of the automatically created Servlet; so how do we somehow initialize or declare something outside of the service method BUT inside the Servlet, in JSP? ->
- To do this, we use "<%! .... %>" which is called the "Declaration" tag.

If you want to import a package, in a traditional Servlet we just write import and then the package name. But how to do this in JSP? ->
- We do this like this: <%@ page import="java.util.Date, java.sql.dfs" %> and this tag is called "Directive" tag.
  ![image](https://github.com/user-attachments/assets/dafd1fba-4ca8-4195-a57a-9d540a8bb8ac)

If you want to print out something, we can do so using -> 
- We do this: <%= k %> Here variable k's value will be printed. This tag is called "Expression" tag. Whatever we write inside the <%= tag will be going inside the out.print() in the automatic converted Servlet.
  ![image](https://github.com/user-attachments/assets/dfdb96f8-727e-4280-9083-85a1b8a3b99d)

#### When to use JSP and when to use Servlet
If your intention is to show data on a page, or create a page, JSP is better. 
If your intention is to process the data, then Servlet is better.

### JSP Directive
1. @page : for importing
2. @include : if you want to use some other jsp file in your current jsp file
3. @taglib : if you want to use extra tags (like spring frameworks' tags)

1. @page
   <%@ page attribute="value" attribute="value" ... %>
   ![image](https://github.com/user-attachments/assets/78338ae5-7a96-4bcc-92a9-8eff01bd8ec3)
2. @inclue
   <%@ include file="filename" %>
   <%@ include file="header.jsp" %>
3. @taglib
   <%@ taglib uri="uri" prefix="fx" %>
   Now when you want to use that tag: <fx:aditya>

### MVC using Servlet and JSP
MVC = Model View Controller
Problems with JSP: 
1. Runs slower in comparison to servlets, because it first gets converted into a servlet.
2. Don't write actual business code inside jsp.

Whenever a client sends a request, it will go to a controller. Its the controller's responsiblity to call the view, and this view will go to the client. So when the controller calls the view, it will also send data along with it. This data will be sent in the form of an object (called model), and the view will then fit this data inside it and then send itself to the client. 
Here, the controller is the Servlet. 
The view is the JSP. 
The model is POJO (Plain Old Java Object).
![image](https://github.com/user-attachments/assets/056b1511-6b16-44a0-9ffb-82c27b58808c)

#### N-Tier Architecture (Optional)
![image](https://github.com/user-attachments/assets/36582487-44d8-4e1c-983a-cca0b8bd84f2)
Basically in this, we use the concept of modularity. Instead of writing the database connection (jdbc logic) inside the servlet, the servlet instead calls another class called service, and the service calls DAO (Data Access Object) which then calls the database. 

### Login using Servlet and JSP
In web servers, login functionality is hard because it follows HTTP protocol, which is stateless. So if you login once, it won't remember that you have logged in, and so we have to use some session management, or you can use cookies also. 
Lets say we have Login page, Welcome page, Videos page, and About us page. 
Upon clicking Login button, we can invoke a Verify.java servlet in which we would set the username attribute in the session, and redirect to the Welcome page (using sendRedirect()). 
Then in Welcome page we will perform a check; whether there is a username key-value pair in session. We will use "if" statement for this: if(session.getAttribute("username")==null){
then redirect to login page
} 
Upon clicking the logout button, a servlet should be invoked Logout.java in which we remove the attribute: session.removeAttribute("username"). 
Do these above steps for every secure page. 

We can do this:  <h1> Welcome ${username} </h1>
here, ${} is called JSTL, and we can pass in the variables from session. 

### How to Prevent Back button after Logout
In your welcome.jsp, do the following:
<% 
  response.setHeader("Cache-Control", "no-cache, no-store, must-revalidate");
  ...
%>
This above code works for HTTP 1.1
![image](https://github.com/user-attachments/assets/179c0ad5-a382-4317-acad-0a66c1277488)

### Incorporating JDBC for fetching username and password from MySQL Database for Validation
- First, get mysql-connector from maven and put that dependency in pom.xml.
- Second, open mysql database
- Third, do JDBC connection in a new package which has class called LoginDao.java:
![image](https://github.com/user-attachments/assets/3c875548-9efa-4503-9cff-785cc7289a0a)
- Lastly, your Login.java should look like this:
  ![image](https://github.com/user-attachments/assets/98da893c-f25b-4b45-8009-7a29fcdc32ab)

### Servlet Filter
Lets say we have a web container which has 3 servlets a, b and c. Now lets say we want to print logs for a and c, maintain security for b and c and so on. Then we would have to write extra repetitive code in them. 
Instead we can use filters which would be code taken out from a, b and c.  
![image](https://github.com/user-attachments/assets/05f0124a-f5e6-4aaf-81e6-b8ad95ca8c79)
To create Servlet filter, we must create a class which extends ServletFilter.

## Extra Notes on Basic Java
- If we have initializer block, static block and constructor in a class, when we call an object, first the static block will be loaded into the jvm and called first, then object will be instantiated and so now the initializer block will be called, after which the constructor block will finally be called. 

### Static in Java
- The main concept behind static is that it belongs to the class rather than instances of the class.
- The static method can not use non-static data member or call non-static method directly.
- "this" and "super" cannot be used in static context.
- static block: executes when a class is loaded into the memory. Syntax:
```
static {
  
}
```
It is usually used to initialize static variables. 
To make sure only one instance is created of a class:
```
public class School{
	private static School school = new School();
	private School(){ //the constructor is private. So now no one outside this class can create an object using this constructor
				
	}
	public static School getInstance(){
		return school;
	}
}
```
Now in another class when we want to create an instance of School, but we want to make sure only one instance can be created:
```
School instance = School.getInstance();
```
- In java, when we declare local variables, their memory is created in stack, and when we do this -> new int(); then that is stored in heap. So if we do int[] arr = new int[4]; Then in stack, arr will be stored. And in heap, we will have 4 integers with their allocated spaces. In the stack, arr will have the first address of the first integer in the heap.  

## File Handling in Java
Stream is a series of data. If you want to store data in byte form, use Byte Stream. If you want to store data in character form, use Character Stream.
#### Create a file
createNewFile(). Returns boolean: true if it successfully creates a new file, and false if file already exists.
#### Get File Information
#### Write to a File
FileWriter class and its write() method.
#### Read from a File
Create an instance of Scanner class, and use hasNextLine() method nextLine() method to get data.
Then close(). 
#### Delete a File

```
File f0 = new File("NewFile.txt"); //Creating an object of a file
if(f0.createNewFile()){
  sout("success");
} else{
  sout("File already exists");
}
if(f0.exists()){
  sout(f0.getName()); //To get file name
  sout(f0.getAbsolutePath()); //Getting path
  sout(f0.length()); //Size of the file in bytes
}
//writing content into a file
FileWriter fwrite = new FileWriter("NewFile.txt");
fwrite.write("Lorem Ipsum");

//closing the stream
fwrite.close();

//Create f1 to read data
File f1 = new File("NewFile.txt");
Scanner dataReader = new Scanner(f1);
while(dataReader.hasNextLine()){
  String fileData = dataReader.nextLine();
}
dataReader.close();

//removing file
File f0 = new File("NewFile.txt");
if(f0.delete()){
  sout("success");
} else{
  sout("error deleting the file");
}
```

Ma'am's code:
```
import java.io.*;
import java.util.*;

public class FileDemo {
	public static void main(String args[]){
		try{  
                       // Creating an object of a file  
                       File f0 = new File("NewFile.txt");   
                       
		       if (f0.createNewFile()) {  
                                  System.out.println("File " + f0.getName() + " is created successfully.");  
                       } 
		       else{  
                                  System.out.println("File is already exist in the directory.");  
                       }  
                           
			if (f0.exists()) {  
            			// Getting file name  
            			System.out.println("The name of the file is: " + f0.getName());  
   
            			// Getting path of the file   
            			System.out.println("The absolute path of the file is: " + f0.getAbsolutePath());     
   
            			// Checking whether the file is writable or not  
            			System.out.println("Is file writeable?: " + f0.canWrite());    
   
            			// Checking whether the file is readable or not  
            			System.out.println("Is file readable " + f0.canRead());    
   
            			// Getting the length of the file in bytes  
            			System.out.println("The size of the file in bytes is: " + f0.length());    
        		} 
			else {  
            			System.out.println("The file does not exist.");  
       			}  
  
				
		} catch (IOException exception) {  
                              System.out.println("An unexpected error is occurred.");  
                              exception.printStackTrace();  
                  }   
         
		//writing content into file
		try {  
        		FileWriter fwrite = new FileWriter("NewFile.txt ");  
        		  
        		fwrite.write("A named location used to store related information is referred to as a File.");   
   
        		// Closing the stream  
        		fwrite.close();   
        		System.out.println("Content is successfully wrote to the file.");  
    		} 
		catch (IOException e) {  
        		System.out.println("Unexpected error occurred");  
        		e.printStackTrace();  
        	}  

		//reading content from the file
		        try {  
            			// Create f1 object of the file to read data  
            			File f1 = new File("NewFile.txt ");    
            			Scanner dataReader = new Scanner(f1);  
            			while (dataReader.hasNextLine()) {  
                			String fileData = dataReader.nextLine();  
                			System.out.println(fileData);  
            			}  
            			dataReader.close();  
        		} 
			catch (FileNotFoundException exception) {  
            			System.out.println("Unexcpected error occurred!");  
            			exception.printStackTrace();  
        		}  
		
		//removing file
		   	File f0 = new File("NewFile.txt");   
    			if (f0.delete()) {   
      				System.out.println(f0.getName()+ " file is deleted successfully.");  
    			} 
			else {  
      				System.out.println("Unexpected error found in deletion of the file.");  
    			}  
	}
}

/* output:

File NewFile.txt  is created successfully.
The name of the file is: NewFile.txt
The absolute path of the file is: C:\java_program\day3\NewFile.txt
Is file writeable?: true
Is file readable true
The size of the file in bytes is: 0
Content is successfully wrote to the file.
A named location used to store related information is referred to as a File.
NewFile.txt file is deleted successfully.
*/
```
