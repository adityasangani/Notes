# Spark
![image](https://github.com/user-attachments/assets/c70a1739-7b39-4c5d-9a99-c24138cfcb59)

## Spark Architecture
- Master/Driver with cluster manager and two daemons.
- Daemons are: Master/Driver process, Worker/Slave process.

YARN (Cluster Manager) is basically an OS for big data applications.
![image](https://github.com/user-attachments/assets/dd09c046-9db6-4928-b770-bb983f4392e3)

We will write python scripts which will be given to the Driver. Driver creates a "Spark Context" (available as "sc") object. Entry point for the execution of spark program is this Spark Context (just like main method in Java). It acts as the bridge between your application and cluster. Driver program (it is like the main method) communicates with Spark through Spark Context.

- Workers have internal daemons called "Executors" which are responsible for executing your code.
- No. of executors to spin up on worker node = No. of CPU cores available on the worker.
- If worker has 4 CPU cores, and 128GB RAM, we can have 4 executors each with 1CPU core and 32GB RAM.

- We will write Spark Application.
Execution Hierarchy of Spark
- Spark Application ---> Jobs ---> Stages ---> Tasks

- Spark is written in Scala, and Scala is written in Java.
- Every list that we create will be local and exist only in a single machine.
- Spark dataframes are distributed.
- Just like how there is list, dictionary, etc. in Python, we have RDD (Resilient Distributed Dataset) in Spark.
An RDD is the fundamental data structure in Spark. It is a fault-tolerant collection of elements that can be processed in parallel across a cluster.

Key Characteristics of RDD:
- Immutable: Once created, it cannot be modified.
- Distributed: Data is split across multiple nodes for parallel computation.
- Lazy Evaluation: Transformations are not executed immediately; they are only computed when an action is triggered.
- Fault-Tolerant: Automatically recovers from failures using lineage (i.e., remembers how it was created).
- Low-Level API: Requires more effort for optimizations.

![image](https://github.com/user-attachments/assets/b9c6f5fd-876c-4542-868e-6c2742768561)

The lower part of the image represents the architecture of Apache Spark:

**SPARK CORE (Foundational Layer)**

This is the main processing engine that provides:
- Task scheduling.
- Memory management.
- Fault recovery.
- RDD abstraction.

**RDD (Low-Level Abstraction)**

RDDs (Resilient Distributed Datasets) are immutable, distributed collections of objects.
They support transformations and actions to process large-scale data.

**Higher-Level Libraries Built on Spark Core:**

**SPARK SQL (Semi-structured Data Processing)**

Allows querying structured and semi-structured data using SQL.
Uses DataFrames and Datasets for optimization.

**SPARK STREAMING (Real-time Data Processing)**

Handles real-time data streams (e.g., from Kafka, Flume, Twitter).
Uses micro-batching to process data continuously.

**MLlib (Machine Learning Library)**

Provides scalable ML algorithms for classification, regression, clustering, etc.

**GraphX (Graph Processing)**

Optimized for graph computation.
Used for social network analysis, PageRank, etc.

## RDD
- Internally, RDDs are partitioned, so that they can be processed efficiently.
- Data is distributed in memory across the worker nodes for parallel computation.
![image](https://github.com/user-attachments/assets/2cc0264d-a5e0-4a30-86c0-bc62108695b4)
- RDD is an immutable collection of objects which computes on the different node of the cluster.
- To modify RDDs, there are only two types of operations: 1. Transformations 2. Actions

### Transformation
- Converts an RDD into another RDD.
- You can transform your RDD as many times as you want.
- These transformations are nothing but methods that you have to call on the RDD.
- The actual data processing doesn't happen immediately.
- It happens only when user requests a result.
