# Hadoop
Guarantees of Hadoop
- High availability 
- Data localisation

Enter process of Hadoop is written in Java.

Cluster can be built using commodity hardware.

HDFS: Java based distributed file system that can store all kinds of data without prior data organization.

Client - Gateway node/ Edge node
Data storage and processing will happen on the slave
Entry point for everything by Master (check this once)

## Modes of Hadoop Cluster Setup
- Pseudo-Distributed Mode is a simulation of the Fully distributed mode. All the client process, master process and slave process will exist in one machine.
- Local Mode - One java process
- Fully distributed mode - All java processes in many machines. (client java process, master java process, slave java processes)

## Cluster Terminologies
1. Cluster :  a collection of computers which works together in the same network.
2. Node : A single individual computer in the cluster.
3. Daemon : A process/program which runs on the node to perform different functions based on the requirement. It is a sleeping process. 

Usually a process' lifespan is short, whereas a daemon is always running.

## HDFS Architecture
1. NameNode : Master
2. DataNode : Slaves
3. Secondary NameNode : Helper node to the master
4. Client Node/Gateway Node: Where all users initiate the data storage and processing tasks. Communicates with HDFS through the fs shell utility.

Configuration Parameters: 
1. Block Size
2. Replication Factor

Data is divided into blocks. Blocks are replicated. 
Why are data blocks replicated? Because Hadoop guarantees High availability. If machine goes down, because of replicated data, it becomes still available.

![image](https://github.com/user-attachments/assets/d8c0bcaf-b952-4673-9723-6aaf53b0d5b1)

We do all this since as my data is too big, it can't be just stored in one machine, that's why I need a distributed cluster.

### DataNode
Serves read and write requests from the file system clients.
Each data communicates with the name node and sends the block report: how much free space, how much utilized space
- This is known as heart beat.
- This info is sent every 3 seconds.
- If the name node does not receive the heart beat the data node is marked as dead.

- Rack: A collection of data nodes connected to the same network switch.
  ![image](https://github.com/user-attachments/assets/ffc754f1-83e8-4e5e-a3e6-d6547c0bfa38)

```
start-dfs.sh
cd
hadoop fs -put /home/ubuntu/dataset/sourcedata/departuredelays.csv demo //to copy file to demo
```

To get first 10 lines of file in hadoop
```
hadoop fs -cat demo/departuredelays.csv | head -n 10 //it is prefixed by /user/ubuntu
```

To do the same without headers: (displaying from the second line outwards)
```
hadoop fs -cat demo/departuredelays.csv | head -n 10 | tail +2 //it is prefixed by /user/ubuntu
```
![image](https://github.com/user-attachments/assets/cd592ef8-ea6b-4607-9554-1446c7eb6df5)

To rename the file in hadoop
```
hadoop fs -mv demo/departuredelays.csv demo/dd.csv
```

To create another directory:
```
hadoop fs -mkdir /user/ubuntu/demo1
```

To copy dd.csv to demo1
```
hadoop fs -cp demo/dd.csv demo1/dd.csv
```

To remove directory with its contents
```
hadoop fs -rm -r demo
```

To put a file from local to distributed:
```
hadoop fs -put /home/ubuntu/dataset/sourcedata/departuredelays.csv demo
```

To get back a file from distributed to local:
```
hadoop fs -get demo1/dd.csv /home/ubuntu/dataset/sourcedata/departuredelays.csv
```

To get the free space available in hdfs of entire file system
```
hadoop fs -df -h
```

To get the free space available in hdfs of a particular file
```
hadoop fs -du -h demo1
```

```
hadoop fs -stat "%n %o %r %b %F" demo1/dd.csv
name logsize replicationfactor filesize filetype
```

q17 - To change the replication factor. by default usually 1 or 3 (check)
```
hadoop fs -setrep -w 3 demo1/dd.csv
```
-w here means wait till the operation is complete.

To remove file
```
hadoop fs -rm demo1/dd.csv
```

hadoop fs -D dfs.blocksize=5M -D dfs.replication=2 -put /home/ubuntu/dataset/sourcedata/departuredelays.csv demo1/dd.csv
![image](https://github.com/user-attachments/assets/3c82c6d2-33d0-4a4b-97e6-cc95b8a1de89)

How do you handle big data?
- hadoop (tries to solve storage and processing)
- Nosql databases (offers storage for unstructured/semi structured data)
- Spark (super-fast in-memory processing)

How does Hadoop solve the 2 problems of Big Data?
- HDFS, Yarn, and MapReduce
HDFS: handles storage. Stores files in blocks and replicates them. 

Hadoop’s guarantees: 
- high availability 
- Fault tolerance
- Data localisation

High availability means being able to access data even in case of system failure. 

Fault Tolerance means being able to resubmitting the job even in case of some issue. Tolerance to fault at the time of execution 

Data localisation: this is what hadoop cluster tries to do. 

When we do hadoop fs -put, the data is stored in the Datanodes 

To execute a program, the data should all come together. It will be a mapreduce program. 
In a traditional distributed system, the data from all places is brought into one system and then program is executed. However in hadoop, a mapreduce program is executed on every machine. The data is not moved anywhere. This is called data localisation.   

What is YARN: manages the resources required for executing a program. Like cpu scheduling, memory etc. 

Data analytics programs submitted to a hadoop cluster is called as Mapreduce jobs.

Which is the node from which you initiate tasks with HDFS? Clientnode

Mapreduce job will first go to the yarn master. 

- Hdfs has master, slave
- Yarn has master (called resource manager), slave (node manager)

Anything related to storage is hdfs, and anything related to execution is yarn. 

The data that we saw in our systems were the datanodes. 

fs shell utility is what we use
hadoop fs : 
To find out where the master node is running, go to localhost:9870, there you will find localhost:9000 which is where the name node is running. 

Taking a look at config files:

gedit $HADOOP_HOME/etc/hadoop/core-site.xml &

We will get a config file. In that, fs.default.name will give us where the namenode is. 

There is one more config file:
gedit /home/ubuntu/bigdata/hadoop/share/doc/hadoop/hadoop-project-dist/hadoop-hdfs/hdfs-default.xml &

dfs.namenode.name.dir
Will have the physical path of the namenode meta data.

dfs.namenode.data.dir 
Will have the physical path of all the data. 

dfs.replication: for replication factor. 
dfs.user.home.dir.prefix
hdfs-site.xml

Namenode metadata is available as two files
1. fs image (master copy of all files)
2. Edit log (stored on the disk of the main machine)

fs image is stored on disk. For faster access, all the recent files added and removed are logged in edit log. Edit files stores the logs of the edits of the **metadata**.

At some point, the fs image and edit log need to be merged. For this, the namenode takes the help of its friend: secondary namenode. 

Edit logs are stored in memory for faster access. 
Checkpointing (from slides)

Secondary namenode is a helper to the primary namenode, not its backup.
Secondary namenode is also called checkpoint node. 


Mapreduce model has 2 phases:
1. map phase (you want to apply some transformations on columns)
2. Reduce phase : mainly about aggregating data (multi table joins)

Every input here is key value pairs. 
In map phase, we have splitter, map, combiner and partitioner. Combiner and Partitioner is optional. 
In reduce phase, we have sort & shuffle, reduce. 

Split -> mapper -> sort and shuffle -> reduce

Splitter is going to split every line by taking every line, and pass everything as a key value. 
Mapper is going to map every word.
Every word will become a key and will be assigned a value of 1.
Next sort and shuffle will be performed so that all the keys are brought together. 
Now mapreduce will send this to yarn for execution. 

Now, java mapreduce we don’t use. We will use Apache Hive wherein, we use direct queries by using hive’s query language, which will internally be converted into mapreduce. 

start-yarn.sh

Optional stages after map:
Combiner: immediately after map
Partitioner:, we have all the keys and values. But too many keys. So the key value data is now partitioned

after sort and shuffle
1 mapper and 1 reducer