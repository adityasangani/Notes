### Hibernate
It is an ORM tool for persistent data.
Steps: 
1. Create a Maven project with archetype "quickstart"
2. Paste the following dependencies in pom.xml
   ```
    <dependency>
      <groupId>org.hibernate.orm</groupId>
      <artifactId>hibernate-core</artifactId>
      <version>6.6.4.Final</version>
    </dependency>
    <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>8.0.33</version>
    </dependency>
   ```
3. In src/main create a resources folder. Right click on this folder, open module settings, click on modules on the left bar, click on sources, go to src/main/resources and mark it as resources and apply.
4. Create a hibernate.cfg.xml file in resources folder, and paste the following code and make changes accordingly:
   ```
   <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE hibernate-configuration PUBLIC
            "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
            "http://hibernate.org/dtd/hibernate-configuration-3.0.dtd">
    <hibernate-configuration>
        <session-factory>
            <property name="hibernate.connection.driver_class">com.mysql.jdbc.Driver</property>
            <property name="hibernate.connection.url">jdbc:mysql://localhost:3306/new_practice</property>
            <property name="hibernate.connection.username">root</property>
            <property name="hibernate.connection.password">root</property>
            <property name="hibernate.connection.pool_size">1</property>
            <property name="hibernate.current_session_context_class">thread</property>
            <property name="hibernate.show_sql">true</property>
            <property name="hibernate.dialect">org.hibernate.dialect.MySQLDialect</property>
            <property name="hibernate.format_sql">true</property>
            <property name="hibernate.hbm2ddl.auto">create</property> //keep it as create in the start. Create will create a new table everytime, and delete and create a table if it already exists. Change this keyword to update once the table has been created
    
    <!--        <mapping resource="book.hbm.xml" />-->
        </session-factory>
    </hibernate-configuration>
   ```
5. In Alien class:
   ```
     package com.aditya;

    import jakarta.persistence.Entity;
    import jakarta.persistence.Id;
    
    @Entity
    public class Alien {
        @Id
        private int aid;
        private String aname;
        private String color;

      public int getAid() {
          return aid;
      }
  
      public void setAid(int aid) {
          this.aid = aid;
      }
  
      public String getAname() {
          return aname;
      }
  
      public void setAname(String aname) {
          this.aname = aname;
      }
  
      public String getColor() {
          return color;
      }
  
      public void setColor(String color) {
          this.color = color;
      }
    }

   ```
6. In App.java file:
  ```
  package com.aditya;

import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.Transaction;
import org.hibernate.cfg.Configuration;

public class App 
{
    public static void main( String[] args )
    {
        Alien telusko = new Alien();
        telusko.setAid(101);
        telusko.setAname("Navin");
        telusko.setColor("Green");

        Configuration con = new Configuration().configure();
        con.addAnnotatedClass(com.aditya.Alien.class);
        SessionFactory sf = con.buildSessionFactory();

        Session session = sf.openSession();
        Transaction tr = session.beginTransaction();
        session.save(telusko);
        tr.commit();
        session.close();
    }
}

  ```
@Entity and @Table are two different things. The Table name is derived from Entity name if @Table is not mentioned. 
We can also change Column name using this: @Column(name="alien_color")

### Fetching Data Using Hibernate
```
  public static void main( String[] args )
    {
        Alien telusko = null;
        Configuration con = new Configuration().configure();
        con.addAnnotatedClass(com.aditya.Alien.class);
        SessionFactory sf = con.buildSessionFactory();

        Session session = sf.openSession();
        Transaction tr = session.beginTransaction();

        telusko = (Alien) session.get(Alien.class, 101);

        tr.commit();
        System.out.println(telusko);

    }
```

### Embeddable
Now lets say we have two classes A and B. We want to create a table of class A, whose one attribute is of type class B. Now, in order to ensure that Hibernate does not create another table for class B also, we write @Embeddable above the class B. 

### Mapping Relations
This is basically on how to add the "Many to one", "One to Many", "Many to Many" and "One to One" relations in Hibernate.

#### For One to Many and Many to One: 
![image](https://github.com/user-attachments/assets/2e142b36-9eb7-4203-b95c-97c665384b0b)
Here, we will still have 2 more tables being created by both these tables: Student will create an extra table called student_laptop, and Laptop will create an extra table called laptop_student. In order to prevent that, make the following changes where applicable: @OneToMany(mappedBy="student") 
![image](https://github.com/user-attachments/assets/97f7df0f-e8c8-4514-be76-c4a036cfedaf)

#### One To One
```
  @Entity
  public class Student {
    @Id
    private int rollno;
    private String name;
    private int marks;
    @OneToOne
    private Laptop laptop;

```

### Fetching: LAZY and EAGER
By default it is lazy. To make it eager: @OneToMany(mappedBy="alien", fetch=FetchType.EAGER)

### Caching
When you want to accesss the same data multiple times, instead of hitting the database multiple times, we can use Caching.
![image](https://github.com/user-attachments/assets/7db82dff-398b-4558-b72e-9826cacfcce8)
If you are firing the same query in the same session, Hibernate provides you First Level Cache. If you have two different sessions, then you can't use First Level Cache. You must then use Second Level Cache.
All the sessions in the same application can share the Second Level Cache. Hibernate provides only First Level Cache. To use Second level, we must configure it using Third Party libraries.
Steps to configure Second Level Cache:
1. Go to maven repository, get ehcache and put in pom.xml.
2. Download jar file hibernate_ehcache jar file.
3. In hibernate.cfg.xml:
     ```
     <property name="hibernate.cache.use_second_level_cache">true</property>
     <property name="hibernate.cache.region.factory_class">org.hibernate.cache.ehcache.EhCacheRegionFactory</property>
     <property name="hibernate.cache.use_query_cache">true</property>
     ```
4. @Cachable, and @Cache(usage=CacheConcurrencyStrategy.READ_ONLY) in Class.

### Hibernate Query Language (HQL)
![image](https://github.com/user-attachments/assets/4d2a8366-21b3-4eca-a432-5b6f6d94eafa)

In SQL: 
select * from student
In HQL: 
from Student

![image](https://github.com/user-attachments/assets/01c7f717-3198-4d3f-8034-fa3e5bfd06ea)

![image](https://github.com/user-attachments/assets/69564f3d-dac3-45e7-b6e3-c1541d9bfdd0)
![image](https://github.com/user-attachments/assets/1305a30d-8f87-42be-a491-373745af298a)
![image](https://github.com/user-attachments/assets/8d9917dc-7833-4706-b7c8-7f0123fed151)
![image](https://github.com/user-attachments/assets/38d266f6-778b-4445-a527-29bd9a7481ff)

### Hibernate Object States | Persistence Life Cycle
For every object in java we atleast have 2 states, New and End state.
Hibernate makes its own states. When we create an object, it becomes a transient state. Transient state means that whenever you destroy that object, you will lose its data. If you want to get that data back, you must persist it. If the object is in the Persistent state, then whatever we do with that object's values will be reflected in the database as well. 
- The next state is Detached. Now lets say we want to perform some operation on the object but don't want that to be reflected in the database. Detached and Transient states are very similar.
- If you want to remove data from database after the Persistent state, for that we have the Removed State.
- If you want to get data from database without creating a new object: get(), find(). We will go directly to the Persistent state.
  ![image](https://github.com/user-attachments/assets/6ab387a3-b44b-4d4d-8007-8ab9fd452894)
- Laptop l = new Laptop(); [New state]
- l.setLid(51); [Transient state. The moment we start setting values, it goes to the Transient state]
  l.setBrand("Sony");
  l.setPrice(700);

session.save(l) [Persistent state]
session.detach(l) [Detach state]

### Difference between Get and Load in Hibernate
- Every time you use get() you will hit the database. Using Get makes more sense if we are fetching data.  
- However, load() will not hit the database. Load will give you the Proxy object (a blank object). Using Load makes sense when we are defining an object that depends on another object.

### JPA (Java Persistence API)
JPA is a specification for ORM tools to follow. 

#### JPA Implementation
Dependencies in pom.xml: hibernate, and mysql connector. 
In order to implement JPA, we need to use find() function from an interface called EntityManager which comes from a class called EntityManagerFactory. 
- In src/main create a folder called "resources" which has a folder called "META-INF". Inside this folder, create a file called "persistence.xml".
  ![image](https://github.com/user-attachments/assets/a09a0db9-b17a-4934-aa2d-eb7c3f95b834)
- Now in App.java:
  ```
     EntityManagerFactory emf = Persistence.createEntityManagerFactory("pu");
     EntityManager em = emf.createEntityManager();
     Alien a = em.find(Alien.class, 4); //to fetch
  //To save: em.persist(a);
  ```
