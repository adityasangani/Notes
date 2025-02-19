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

3. Creating RDD from the Python collection
```
l = [1,2,3,4,5,6,7,8]
rdd0 = sc.parallelize(l) //creating a rdd
rdd1 = rdd0.map(lambda x: x + 1) //increment each element by 1
rdd2 = rdd1.filter(lambda x : x%2==0) //filter for even numbers
rdd3 = rdd2.map(lambda x: x**2) //find the square of the numbers
rdd3.collect() //to display the content onto the screen. collect() is an action. all the partitions from the worker nodes will be passed to the Driver. It will trigger entire chain of transformation.
rdd3.saveAsTextFile() //to persist it by saving it as a text file.
rdd3.sum() //shows us the sum of the elements. It is an action.
rdd3.count() //shows number of elements. It is an action.
rdd3.take(5) //shows 5 elements. It is an action.
rdd3.first() //shows us the first element. It is an action.
```

4. Now lets create RDD from file source
```
words_rdd = sc.textFile('file:///home/ubuntu/wordcount_sample.txt') //if you skip 'file:// then it will use hdfs file system. so don't forget to include it. this command creates rdd for a text file.
words_rdd.collect()
tokens_rdd = words_rdd.flatMap(lambda line: line.split(' '))
tokens_rdd.collect()
mapped_words_rdd = tokens_rdd.map(lambda token: (token, 1))
mapped_words_rdd.collect()
word_count_rdd = mapped_words_rdd.reduceByKey(lambda x,y: x+y) //reduceByKey - internally executes groupByKey. reduceByKey groups values by their keys (words in this case) and applies the given function (lambda a, b: a + b) to combine the values. The function adds up the counts for the same word.
for i in word_count_rdd.collect():
  print(i)
```

sc.parallelize and sc.textFile can take a second argument in which we can specify the number of partitions.
```
l = [1,2,3,4]
rdd0 = sc.parallelize(l, 2)
rdd0.getNumPartitions()
rdd0.toDebugString()
```

pairRDD is a tuple of size 2 -> (key, value) waale.
