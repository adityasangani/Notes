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
