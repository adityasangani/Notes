# Cassandra
- It is a distributed database.
- Distributed means: database is available in multiple computers at once.
- It has fast writes.
- It is a columnar-oriented database.

## Column Oriented Database
  - They tend to store each column on a separate file on the disk. Means, Name ka ek file hoga, Age ka ek file hoga, and so on. So here, we can only access columns.
  - In a Relational Database, if we want to access a column Name, we would have to fetch every row of every column.
  - Each column has a key-value pair. For example, Name column has: {Name: Brian}, {Name: John}, etc.
  - We can see that in the Car column, in the end there is no value stored there. Not even null. This is fine in Cassandra, but not fine in RDBMS. In RDBMS we have to store null value.
![image](https://github.com/user-attachments/assets/e4a1bd5e-96b2-4053-a468-d86773a13f93)

## CAP Theorem
These are the properties that a distributed system or database have:
1. Consistent: Every node always returns the same most recently written data. 
2. Available: Every non failing node returns a response to any read or write request in a reasonable period of time. This means that the database should always be available for us to get the data, or give data to it. 
3. Partition Tolerant: This means that the system will continue to function even during a network partition or failure. A network partition is when some of the nodes cannot talk to each other.
The theorem states that at any one time, a system can have only any 2 of these attributes, never three.
![image](https://github.com/user-attachments/assets/1f71888c-ed3c-49b2-9cf6-e5706a0eb580)
We can see in the Venn diagram that there is no instance where all three meet.

In a distributed system, we MUST have Partition Tolerance. So what's left to decide is between Consistent and Available.
![image](https://github.com/user-attachments/assets/09c7410b-4fd6-42ae-9090-2408c7b47c63)

- Cassandra runs on JRE.
- 
  
