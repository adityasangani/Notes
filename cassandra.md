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

In Cassandra, the primary key consists of 1. Partition Key (determines which node stores the data)  2. Clustering Key (determines how data is ordered within a partition)
2. How to Choose the Partition Key?
✅ Partition Key MUST be chosen to:
1️⃣ Distribute data evenly across nodes (avoid overloading a single node).
2️⃣ Support fast lookups (fetch all relevant data efficiently).
3️⃣ Ensure queries don’t require scanning the entire cluster.

🚨 Partition Key Pitfalls to Avoid:
❌ Too Broad (e.g., a single partition for millions of rows → too much data on one node).
❌ Too Specific (e.g., one partition per row → defeats partitioning, no benefits).

 How to Choose the Clustering Key?
✅ Clustering Key MUST be chosen to:
1️⃣ Sort data within a partition (e.g., chronological order).
2️⃣ Support range queries (e.g., "fetch all purchases between 2022 and 2023").
3️⃣ Allow efficient filtering (e.g., "latest entry first").

🚨 Clustering Key Pitfalls to Avoid:
❌ Missing a Clustering Key when multiple rows per partition are needed.
❌ Wrong sorting order (Cassandra stores clustering keys in ASC order by default).

📌 Syntax:
```
PRIMARY KEY ((partition_key), clustering_key_1, clustering_key_2, ...)
```
Example 1: Designing a Table for an E-commerce Order System
💡 Use Case:

Store orders placed by users, retrieving all orders by a specific user efficiently.
Support fetching recent orders first.
✅ Schema Choice:
```
CREATE TABLE orders (
    user_id UUID,          -- Partition Key (Distributes data per user)
    order_time TIMESTAMP,  -- Clustering Key (Sorts orders per user)
    order_id UUID,
    item TEXT,
    total_amount DECIMAL,
    PRIMARY KEY ((user_id), order_time)
);
```
Why?
- Partition Key (user_id) → Groups all orders for a user together.
- Clustering Key (order_time) → Orders purchases chronologically per user.

![image](https://github.com/user-attachments/assets/c5d4ef6d-0d46-46ab-8f67-3ca0de441e19)



## Source Command
- Source commands allows you to execute a set of CQL statements from a file.
- The file name must be enclosed in single quotes: ```SOURCE './myscript.cql';```
- cqlsh will output the results of each command sequentially as it executes.

## Nodes and Configuration
- Node: One cassandra instance
- Rack: A logical set of nodes
- Data Center: a logical set of racks
- Cluster: the full set of nodes which map a single complete ring.
![image](https://github.com/user-attachments/assets/59122c0a-c408-4a60-8f08-6fbf39bc2cb0)

### Cluster
Nodes join a cluster based on the configuration of their own conf/cassandra.yaml file.
Key settings- 
1. cluster_name: shared name to logically distinguish a set of nodes.
2. seeds: IP addresses of initial nodes for a new node to contact and discover the cluster topology.
3. listen_address: IP address through which this particular node communicates.

#### Node
- Node has a JVM and it runs a Java process. This Java process is called Cassandra Instance.
- All data that a node stores will be in a distributed hash table.

### Coordinator and Partitioner In Cassandra
1. Coordinator:
A coordinator is the node that receives a client request (read/write) and is responsible for routing it to the appropriate nodes in the cluster.
- When a client connects to any Cassandra node, that node becomes the coordinator for the request.
Example:
Assume we have a 3-node Cassandra cluster with data distributed across them.
- The client sends a request to Node A.
- Node A becomes the coordinator.
- It determines that the data is stored on Node B and Node C.
- If it's a write request, Node A forwards the request to both nodes.
- If it's a read request, Node A fetches the data from the fastest node and sends it to the client.
The coordinator manages the Replication_Factor (RF).
- RF = Onto how many nodes should a write be copied?
- RF is set for an entire keyspace, or for each data center, if multiple data centers are present.
- SimpleStrategy: one factor for entire cluster.
- NetworkTopologyStrategy: separate factor for each data center in cluster.
### Consistency
- Consistenct Level: sets how many of the nodes to be sent a given request must acknowledge that request for a response to be returned to the client.

### Gossip Protocol
- Once per second, each node contacts 1 to 3 other nodes, requesting and sharing updates about:
  1. Known node states (heartbeat)
  2. Known node locations
  3. Requests and acknowledgements are timestamped, so info is continually uploaded and discarded.
With this, reliably and efficiently spreads node metadata through the cluster.

2. What is a Partitioner?
A partitioner is responsible for determining how data is distributed across nodes. It decides which node stores a particular row by computing a hash of the partition key.

## Insert Syntax
- Requires a value for each component of the primary key, but not for any other columns.
- Primary Key columns are mandatory.
- Missing values are set to null.
```
INSERT INTO table_name (column_list)
VALUES (column_values)
```

## ALTER Syntax
```
ALTER TABLE table1 ADD another_column text; //adding a column
ALTER TABLE table1 DROP another_column; //dropping a column. primary key columns are not supported.
ALTER TABLE table1 TYPE FLOAT; //changing a column datatype
```

Using Alter keyword we can: 
- change datatype of column
- add columns
- drop columns
- rename columns
- change table properties
- WE CANNOT CHANGE THE PRIMARY KEY COLUMNS

## UPDATE Syntax
```
UPDATE table_name SET col_name1=value1, col_name2=val2 WHERE primary_key_col=val3;
```
- Rows must be identified by values in primary key columns.
- Primary key columns cannot be updated.
- An existing value is replaced with a new value. A new value is added if a value for a column did not exist before.

## TRUNCATE Syntax
- Truncate removes all rows in table.
- The table schema is not affected.
```
TRUNCATE TABLE movies;
```

## Data Types for Flexibility
1. Collections
2. Counters
3. User Defined Types
These data types simplify table design, optimize table functionality, store data more efficiently, and might change table designs completely.

### Collections
- Used to store multiple values within a single column.
- These are useful when you need to store lists, sets or key-value pairs (Map).
#### List
- Stores values in order and allows duplicates.
```
CREATE TABLE users (
    user_id UUID PRIMARY KEY,
    name TEXT,
    login_ips LIST<TEXT>
);
```


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
5. drop table shopping_cart;
6. 
7. insert into my_keyspace.shopping_cart(userid, item_count, last_update_timestamp) values ('9876', 2, toTimeStamp(now())); 
