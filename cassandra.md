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

- Joins and stuff isn't very efficient in distributed systems because one table would be one server, and the other table would be in another server.
- In Cassandra, we don't use joins. We do Query-first Approach.

## Query First Approach
We design our tables for a specific query. Some consequences might be that we might write the same data to multiple tables. 

In RDBMS:
![image](https://github.com/user-attachments/assets/f35863cd-fd9f-4867-8070-86030bed5d3b)
In Cassandra:
![image](https://github.com/user-attachments/assets/92acb824-54fc-4a39-839a-9a2cbbb59955)
Cassandra Tables:
![image](https://github.com/user-attachments/assets/6b4d4367-b8a2-48a2-8c55-37bd4c9b8418)

Each row in Cassandra represents one entity (just like in MySQL).
However, the key difference is that in Cassandra, each row can have a different set of columns, whereas in MySQL, every row must have the same fixed columns.
- Cassandra allows flexible columns per row, whereas MySQL does not.

## Partition Key
Every piece of data with the same partition key will be stored on the same node in the cluster. 
So in the above example, our partition key would be the CAR MAKE (BMW, Audi) etc. In the second table, our partition key would be Id. 
- In Cassandra, we should only access data using the PARTITION KEY.
- If we want to access data using PRIMARY KEY, we must just create another table.

How does Cassandra achieve this partition?
- For each partition key, Cassandra passes it through a hash function. The purpose of the hash function is to convert the partition key into a unique id.
![image](https://github.com/user-attachments/assets/5306d232-47e3-4fed-8f33-dbf53de5ce45)
These tokens are 64 bit integers.

The values that come out are called Tokens. Cassandra uses these tokens to decide which data will be stored in which node. Now how does it do it?

-> Using the Cassandra ring diagram. Each node will be assigned a token, and it will be responsible for storing data less than the value of that token, but greater than the value of the token assigned to the previous node. 

![image](https://github.com/user-attachments/assets/ae0dda2a-c1d8-4fa9-acef-f8debd2b9403)
- Each large rectangle represents a data center.
- A rack in a data center is basically a cluster of connected machines. 

If replication factor=3, this means that we want our data in our database to be stored on three separate data nodes. 
- Simple Strategy: We simply find the token for the record we're trying to add it, and add the token to the token range that it would fall in. 

Commands:
1. cqlsh
2. describe keyspaces;
3. create keyspace my_keyspace with replication={'class':'SimpleStrategy', 'replication_factor':'1'} AND durable_writes='true'; (by default durable_writes will be true only. If we set it to false, 
4. create table if not exists my_keyspace.shopping_cart (
   userid text primary key,
   item_count int,
   last_update_timestamp timestamp
   )
6. insert into my_keyspace.shopping_cart(userid, item_count, last_update_timestamp) values ('9876', 2, toTimeStamp(now())); 
