# Database
- It is a physical container for collections.
- A single MongoDB server has multiple databases.

# Collection (eq. table)
- It is a group of MongoDB documents. It is equivalent to RDBMS table.
  
# Document
- It is a set of key-value pairs.
- MongoDB stores BSON documents, i.e. data records in collections; the collections in databases.

![image](https://github.com/user-attachments/assets/db626809-d384-4a8e-87ee-aa61dcccaf54)

# ObjectID
- Small, ordered.
- 12 bytes. First 4 bytes are a timestamp.
- In MongoDB, each document stored in a collection requires a unique _id field that acts as a primary key.

# Projection
- find() method accepts second optional parameter that is list of fields to display.
- 1 is used to show the field
- 0 is used to hide the fields.
```
db.COLLECTION_NAME.find({}, {KEY:1})
```
- _id field is always displayed while executing find() method, if you don't want this field then you need to set it as 0.

# Limit and Skip
```
db.COLLECTION_NAME.find().limit(NUMBER).skip(NUMBER)
```

# SORT Records
- 1 is used for ascending order.
- -1 is used for descending order.
```
db.COLLECTION_NAME.find().sort({KEY:1})
db.users.find().sort({city:1, age:-1})
```

# Dot Notation
- contribs: ["Turing machine", "Turing test", "Turingery"]
- To specify the third element in the contribs array, "contribs.2".
![image](https://github.com/user-attachments/assets/42fa8651-0048-4769-9676-8ddf8651bf0f)

# _ID Field in MongoDB Document
- _id field is always the first field in the documents. If the server receives a document that does not have _id field first, then the server will move the field to the beginning.

# Start MONGODB Service
- By default the mongod process uses the /data/db directory.
- In one terminal ```mongod``` and in another terminal ```mongo```.
- To stop mongoDB, press Ctrl + C in the mongod terminal.
- If you end a line with (, {, [, then you can write in the next lines until you close them with the corresponding ), }, ].
## Basic Commands
```
show dbs //show databases
use mydb //if mydb database doesn't exist, mongoDB will create it when you insert data
db.createCollection("myTestDB") //create a collection
show collections //to see all the collections
db.myTestDB.insertOne({name:"John", age:30}) //inserting a document
db.myTestDB.insertMany([{name:"David", age:24},{name:"Mariah", age:56}]) //inserting many documents
db.myTestDB.find() //finds all documents
db.myTestDB.find({name:"John"}) //find documents where name is "John"
db.myTestDB.updateOne({name:"John"}, {$set:{age:88}}) //updating a document
db.myTestDB.deleteOne({name:"John"}) //delete a document
db.myTestDB.drop() //drop a collection
db.dropDatabase() //drop a database
```

## Querying Data
```
db.myTestDB.find({age:{$gt:25}}) //find those with age greater than 25
```

Comparison Operators
- ```$gt``` : Greater than
- ```$lt``` : Less than
- ```$eq``` : Equal to
- ```$ne``` : Not equal to
- ```$gte``` : Greater than or equal to
- ```$lte``` : Less than or equal to

```
db.myTestDB.find({ $or: [{age:25}, {name:"John"}] })
```

Logical Operators
- ```$and``` : Logical AND
- ```$or``` : Logical OR
- ```$not``` : Logical NOT
- ```$nor``` : Logical NOR

Array Operators
- ```$in``` : Matches any value in an array
- ```$nin``` : Matches none of the values in an array
- ```$all``` : Matches all values in an array

```
db.myTestDB.find({name: {$in: ["John", "Hopkin"]}})
```

Querying an Array
Use $elemMatch operator to specify multiple criteria on the elements of an array such that at least one array element satisfies all the specified criteria.
```
db.myTestDB.find({age: {$elemMatch: {$gt:22, $lt:30}}})
```

## Indexing
An index in MongoDB is like the index in a book—it helps MongoDB find data faster. Instead of scanning the entire collection, MongoDB uses indexes to efficiently locate documents.

👉 Without an index: MongoDB scans all documents (called a collection scan).
👉 With an index: MongoDB quickly looks up the required documents, making queries much faster.

### Creating an Index
By default, MongoDB creates an _id index on every document. But we can create our own indexes to speed up queries.


# Nodes
In simple terms, nodes in MongoDB are just servers (whether physical, virtual, or cloud-based) that run instances of the MongoDB database software. These nodes store, manage, and process data, and they can work together as part of a larger database system to provide features like data redundancy, scalability, and fault tolerance.
- A node = A server running MongoDB.
- Nodes can operate independently or collaborate with other nodes to form a replica set or sharded cluster for better performance and reliability.

In the context of MongoDB, nodes refer to individual instances of the MongoDB server (mongod) that work together as part of a larger database architecture. These nodes can function independently or as part of a replica set or sharded cluster, depending on the configuration.

Here’s an explanation of the different types of nodes:

## 1. Types of Nodes in MongoDB
### a. Standalone Node
A standalone node is a single instance of MongoDB.
It is not part of a replica set or sharded cluster.
Used for development, testing, or simple applications where high availability is not required.
### b. Replica Set Nodes
A replica set is a group of MongoDB nodes that maintain the same dataset to ensure high availability and data redundancy.

**Primary Node**:
Handles all write and read operations by default.
There is only one primary node in a replica set at any time.

**Secondary Nodes**:
Maintain copies of the data from the primary node.
Can handle read operations if readPreference is set to allow it.
Participate in electing a new primary if the current primary fails.

**Arbiter Node**:
Does not store data or participate in replication.
Used to break ties during elections to decide the new primary node.
Useful when an even number of nodes are in the replica set.

### c. Sharded Cluster Nodes
A sharded cluster splits data across multiple shards to enable horizontal scaling.

**Shard Nodes**:
Hold subsets of the data (data is distributed based on a shard key).
Each shard can be a standalone node or a replica set for redundancy.

**Config Server Nodes**:
Store metadata about the sharded cluster, including information about the shards and the distribution of data.
Usually deployed as a replica set for reliability.

**Mongos Nodes**:
Act as query routers for the sharded cluster.
Direct client queries to the appropriate shard(s) based on the metadata from config servers.

## 2. Node Communication and Roles
Replication: Nodes in a replica set continuously sync data to maintain identical datasets.
Election Process: If a primary node in a replica set fails, secondary nodes hold an election to promote a new primary.
Shard Distribution: Mongos routes queries to the correct shard based on the data's shard key.

## 3. Examples of Nodes in Different Architectures
Replica Set Example:
Node Role	Description	Data Stored?
Primary Node	Handles all writes and reads	Yes
Secondary Node	Syncs with the primary	Yes
Arbiter Node	Breaks election ties	No
Sharded Cluster Example:
Node Role	Description	Data Stored?
Shard Node	Holds a subset of the database data	Yes
Config Server	Stores metadata about the cluster	Yes
Mongos Router	Directs client queries to the appropriate shard	No
Why Are Nodes Important?
Scalability: Adding more nodes allows the database to handle larger datasets and more traffic.
Fault Tolerance: Replica sets ensure data availability even if some nodes fail.
Performance: Sharded clusters distribute data, enabling faster query execution.

Cluster means it will have multiple nodes (means multiple servers across the regions (check locations of nodes)). Due to this, Replication and Sharding will be possible.

- Replication: Same data's copy will be present in other servers. This is so that if one server goes down, then the request can be sent to the other server instead. 
- Sharding: Data will be stored in a distributed manner. Let's say students names are from A to Z, and we have two servers. Then we will evenly distribute the students' names on the basis of a key (which will be their name). 
Lets say our database has 10 students, 5 starting with A, and 5 with B. Then A waale students will be in one server, and B waale students will be on another server.

Why is Sharding Needed?
When a database grows too large, a single machine may struggle with:

1. Storage Limits – A single server might not be able to store all data.
2. Slow Queries – Too much data on one machine slows down reads/writes.
3. High Load – Too many users accessing a single database can create bottlenecks.
Sharding solves these issues by splitting data across multiple shards (servers), allowing: 
✅ Parallel processing of queries
✅ Efficient storage distribution
✅ Faster reads & writes

Types of Sharding
1. Range-Based Sharding: Data is divided based on a range of values (e.g., User ID 1-1000, 1001-2000, etc.).
Easy to implement but shards may become unbalanced if some ranges grow faster.

2. Hash-Based Sharding: Uses a hash function to distribute data across shards randomly.
Prevents hotspots (uneven load) but requires consistent hashing for scalability.

3. Geographical Sharding: Data is sharded based on location (e.g., users in the US on one shard, Europe on another).
Good for reducing latency in global applications.

### Replica Set in MongoDB
A replica set in MongoDB is a group of MongoDB servers that maintain the same dataset, ensuring high availability and fault tolerance. 

#### How It Works
A replica set consists of multiple MongoDB nodes: 
1. Primary Node - Handles all write operations.
2. Secondary Nodes - Synchronize data from the primary (used for read operations and failover).
3. Arbiter (Optional) - Participates in elections (?) but does not store data.

If the primary node fails, a secondary node is elected as the new primary. 
When the old primary recovers, it rejoins as a secondary.

#### Setting Up a Replica Set
The below procedure is to set up a single replica set with 1 primary node and 2 secondary nodes. This works if you're only setting up replication (not sharding).

**Step 1: Start MongoDB Nodes with Replica Set Enabled** 

Run the following for each node:
```
mongod --replSet myReplicaSet --port 27017 --dbpath /data/rs1
mongod --replSet myReplicaSet --port 27018 --dbpath /data/rs2
mongod --replSet myReplicaSet --port 27019 --dbpath /data/rs3
```

**Step 2: Connect to a MongoDB Instance**

Open a MongoDB shell:
```
mongo --port 27017
```

**Step 3: Initialize the Replica Set**

Inside the Mongo shell, run:
```
rs.initiate({
  _id: "myReplicaSet",
  members: [
    { _id: 0, host: "localhost:27017" },
    { _id: 1, host: "localhost:27018" },
    { _id: 2, host: "localhost:27019" }
  ]
})
```
Finally, check the replica set status, which should show us the primary and secondary nodes: 
```
rs.status()
```

### Sharding
The above procedure works if you're only setting up replication (not sharding).

However, sharding requires multiple replica sets.
- Instead of creating one big replica set, we need **one replica set per shard** (e.g., shard1, shard2).
- Each shard (e.g., shard1 and shard2) has its own primary and secondaries.
- Then, we use mongos to distribute queries across these shards.

When to use only that one replication set?
✅ If you only need replication (high availability, failover).
❌ If you need sharding, you'd still need to split the data across multiple replica sets.

Order for a Sharded Cluster:
1️⃣ Set up replica sets for each shard separately (shard1, shard2, etc.).
2️⃣ Set up the config server (which tracks shard metadata).
3️⃣ Start mongos router to connect everything.
4️⃣ Add shards & enable sharding.

#### 1️⃣ Set up replica sets for each shard separately (shard1, shard2, etc.).

1. Start MongoDB Nodes for Each Shard
Each shard should be a separate replica set. Here, we create two shards (shard1 and shard2) with three nodes each.
##### Shard 1 (Replica Set: shard1)
Run these on separate terminals (or servers):
```
mongod --replSet shard1 --port 27017 --dbpath /data/shard1-1
mongod --replSet shard1 --port 27018 --dbpath /data/shard1-2
mongod --replSet shard1 --port 27019 --dbpath /data/shard1-3
```

##### Shard 2 (Replica Set: shard2)
Run these on separate terminals (or servers):
```
mongod --replSet shard2 --port 27020 --dbpath /data/shard2-1
mongod --replSet shard2 --port 27021 --dbpath /data/shard2-2
mongod --replSet shard2 --port 27022 --dbpath /data/shard2-3
```

2. Initialize the Replica Sets
Now, connect to the first node of each shard and configure the replica sets.
##### Initialize Shard 1 (shard1)
Connect to shard1's first node:
```
mongo --port 27017
```

Then run:
```
rs.initiate({
  _id: "shard1",
  members: [
    { _id: 0, host: "localhost:27017" },
    { _id: 1, host: "localhost:27018" },
    { _id: 2, host: "localhost:27019" }
  ]
})
```

Check status: 
```
rs.status()
```

##### Initialize Shard 2 (shard2)
Connect to shard2's first node:
```
mongo --port 27020
```

Then run: 
```
rs.initiate({
  _id: "shard2",
  members: [
    { _id: 0, host: "localhost:27020" },
    { _id: 1, host: "localhost:27021" },
    { _id: 2, host: "localhost:27022" }
  ]
})
```

Check the status: 
```
rs.status()
```

3. Verify Replica Sets
Each replica set should have one primary and two secondaries.
Run the following on each replica set’s primary node:
```
rs.status()
```
You should see one PRIMARY and two SECONDARY nodes.

#### 2️⃣ Start the Config Server
The Config Server stores metadata about the shards.
```
mongod --configsvr --replSet configReplSet --port 27017 --dbpath /data/configdb
```

Initiate the Config Server Replica Set:
```
mongo --port 27017
```

```
rs.initiate({
  _id: "configReplSet",
  members: [{ _id: 0, host: "localhost:27017" }]
})
```
#### 3️⃣ Start the Mongos Router
Now, start the Mongos Router that will direct queries to the correct shard:

```
mongos --configdb configReplSet/localhost:27017 --port 27025
```
#### 4️⃣ Add Shards to the Cluster
Connect to the mongos shell:

```
mongo --port 27025
```

Add the shards:
```
sh.addShard("shard1/localhost:27018")
sh.addShard("shard2/localhost:27021")
```

Check the shards:
```
sh.status()
```

#### 5️⃣ Enable Sharding for a Database & Collection
Enable sharding for a database, e.g., myDatabase:

```
sh.enableSharding("myDatabase")
```

Shard a collection, e.g., users, using hashed sharding:
```
sh.shardCollection("myDatabase.users", { "userId": "hashed" })
```

#### 6️⃣ Final Check
Run:
```
sh.status()
```

It should show: 
✅ Shards added
✅ Sharded database & collection


