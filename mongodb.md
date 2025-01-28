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
