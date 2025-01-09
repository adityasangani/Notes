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
