# Spark Commands

1. In one tab:
```
start-master.sh
start-slaves.sh
jps
```

2. In second tab:
```
pyspark
```
- start-master.sh
Starts the Spark master node (central coordinator).
Runs a web UI on http://localhost:8080 where you can see the cluster status.
Prints the Spark master URL (e.g., spark://<your-hostname>:7077), which you'll need for connecting workers.

- start-slaves.sh
Starts the worker nodes (slaves).
These workers register themselves with the Spark master and start executing jobs when requested.

- jps (Java Process Status)
Lists all running Java processes.
Useful for verifying if the Master and Workers are running properly.

```
sc //gives us Spark Context reference
sc.appName //gives us the Application name
dir(sc) //gives us the available methods and attributes for sc
```

even_list = [x : 
