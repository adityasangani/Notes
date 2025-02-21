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
- Whenever we take in a textFile into RDD, by default the data type of the values is string.

### Transformation
- Converts an RDD into another RDD.
- You can transform your RDD as many times as you want.
- These transformations are nothing but methods that you have to call on the RDD.
- The actual data processing doesn't happen immediately.
- It happens only when user requests a result.

# Spark SQL
- Spark SQL operates with Catalyst Optimizer (Tungsten Project).
- We can use Spark SQL when we want to process structured data.
- Ways to interact with Spark SQL:
    - Dataframe API (using python)
    - SQL <br>
MySQL = OLTP <br>
Redshift, Hive, BigQuery, SparkSQL = OLAP


Entry point into Spark Core functionality = Spark Context
Entry point into Spark SQL functionality = Spark Session
![image](https://github.com/user-attachments/assets/4e9e7e51-8a68-427b-8ca3-21c4018ac6a0)

We already have Spark Session available as "spark" in REPL.

## Dataframes
- Similar to tables
- Distributed (partitioned just like RDDs) collection of data organized into named columns.
- Can be constructed from structured data files, existing RDDs.

![image](https://github.com/user-attachments/assets/6c9e49f2-2187-4d64-9f8f-17451c28515c)
![image](https://github.com/user-attachments/assets/172fbd0e-48fe-40b9-8a72-8156ea087ec1)

```
location_schema = "IP string, Country string, State string, City string, ApproxLat string, ApproxLng string"
location_df = spark.read.schema(location_schema).csv('file:///home/ubuntu/dataset/sourcedata/webclicksdata/goShopping_IpLookup.txt', sep=',', inferSchema=True)
location_df.show(5, truncate=False)
location_df.printSchema()
```

Two types of transformations: narrow (map, filter) and wide (groupby, reduceby).  
1 stage will be broken down into --> no of partitions = no of tasks.  

Spark Ecosystem -
1. spark core: low level API - RDD - immutable, lineage - toDebugString()
- Tx - lazily evaluated - DAG - reducebyKey
- Actions - Immediately evaluated - count, sum, take, reduce
- Create RDD - 1. sc.parallelize (python list, <no of partition>)
            - 2. sc.textFile(pathoffile, <min no of partition>
            - 3. rdd1 = derive rdd0
            - 4. dataframe.rdd
- Structure of data inside RDD - ["",""], [(),()]
- Execution of standalone app - spark-submit

2. spark SQL: entry point = spark session
- dataframe: create df - 1. spark.createDataFrame(python list of tuples/dict/RDD, <schema>)
    schema = [col1, col2] or "col1 string, col2 int" or StructType([StructField("col1", StringType(), True), StructField("col2", StringType(), True)]). 
            2. creating df from external sources : spark.read.format("csv").schema().option().load(<path>)
or - df = spark.read.csv("path", sep="\t", header=True, inferSchema=True)
- df = spark.read.json("path")
3. rdd.toDf() but here RDD should be in the apppropriate format. 

DF needs to be registered - Create view and execute SQL queries. 

# Spark Streaming

Spark Streaming- it processes live streaming data by processing it in micro batches. like it will take in input data for 10 seconds, and then process it that batch of data. so there is very less latency but latency is still there. 

![image](https://github.com/user-attachments/assets/017f2e4e-412f-4c59-ab6c-550558313a66)

- If we do the normal groupBy, we will have to update our count every time, after each new batch comes. This is very difficult.
- Hence we don't need to maintain the state end to end.
- In the third row, we see a new way to do this. It keeps count only until 10 seconds. Then its count is reset, and it starts counting again. This concept is called "windowing". We have to configure the required window length based on our use case. Spark requires information about the time in order to execute this. For this, we have something called the "event time" which is the time at which the event is generated.
- So, (event, eventtime) is supplied to spark (event here is the data).
```
groupBy(window(timecolumnname, "10 seconds").groupingColumnname).count()
```
![image](https://github.com/user-attachments/assets/2a54d86b-929b-4369-99ef-f540597ed5fa)
![image](https://github.com/user-attachments/assets/00efdde6-c87e-4a56-9a44-9f7c8e4ce580)


