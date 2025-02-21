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

If we are using a .py file instead of using the REPL, then we must declare SparkContext initially by ourselves.
```
import os
from 
```
Execute it outside the pyspark shell.

# Spark SQL
```
spark.createDataFrame([('Alice', 1)]).show() //here it self assigns the column names as __1, __2.
spark.createDataFrame([('Alice', 1)], ['name','age']).show()
```

![image](https://github.com/user-attachments/assets/c325f379-940d-4d29-b6e2-163e32c6f144)

[pyspark.sql.SparkSession.createDataFrame — PySpark 3.5.4 documentation](https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/api/pyspark.sql.SparkSession.createDataFrame.html)

Creating DF using different data sources - files/databases
- We need a dataframe reader object (using the Spark Session object). ```spark.read (returns dataframe reader object)```
- We need a dataframe writer object to persist dataframe to files/external sources ```df.write```
- When we load data from Parquet or Json, no need to specify schema as it is already evident.

![image](https://github.com/user-attachments/assets/e7e7481e-95c8-4249-9f94-50ee2f7bb587)

For json files:
```
df = spark.read.json('file:///home/ubuntu/Desktop/employee.json')
df = spark.read.format('json').load('file:///home/ubuntu/Desktop/employee.json')
//both above give same result

```

For csv/any delimited files
```
dfcsv = spark.read.option("delimiter","\t").csv('file:///home/ubuntu/dataset/sourcedata/webclicksdata/goShopping_webclicks2.dat')
dfcsv.show(5)
```

Select:
![image](https://github.com/user-attachments/assets/67ecd9ed-2e5b-4cdb-aa67-5ae6089dd149)
![image](https://github.com/user-attachments/assets/672b522d-4ce3-4567-aa7d-ffd45aa4bf52)
![image](https://github.com/user-attachments/assets/b83945af-7501-4036-987a-a395605488cc)

❌ df_webclicks.count().show() → Error (because count() returns an int)
✅ df_iplookup.groupBy("country").agg(count("customer_id")).show() → Works (because it returns a DataFrame)
1️) Why Does df_webclicks.count().show() Give an Error?
```
print(df_webclicks.count())  # Works correctly
```
df_webclicks.count() returns an integer, which represents the total number of rows in the DataFrame.
Integers do not have .show() because .show() is a method of a DataFrame, not an integer.
Hence, df_webclicks.count().show() throws an error.

2️) Why Does .show() Work for Grouped Count?
```
df_iplookup.groupBy("country").agg(count("customer_id").alias("num_customers")).show()
```
Here, groupBy() creates a new DataFrame.
agg(count("customer_id")) counts the customer_id occurrences for each country, returning a DataFrame, not an integer.
Since .show() works on DataFrames, this command runs without error.

![image](https://github.com/user-attachments/assets/7d4a4cec-0f1c-40dc-b244-6a3dcc3b2730)


## Views
Creating views from Dataframes
- it is temporary
```
df.createOrReplaceTempView("empview");
//now to query this view
spark.sql("select empno from empview") //this will give us dataframe object
spark.sql("select empno from empview").show()
spark.sql("select empno from empview").write('file:///home/ubuntu/e_out');
spark.sql("select empno from empview").write.format('json').save('file:///home/ubuntu/e_out');
```

## Spark Streaming
3 tabs
Tab1 (data source): This will be our data source. Always stop this netcat server using Ctrl+C, and not closing the tab directly (if you close tab directly, then restart VM). 
```
nc -lk 9999
//now keep writing anything. Once we hit enter, all of what we have written will be available in that port number 9999
```
Tab2: Spark Streaming app:
```
lines = spark.readStream.format("socket").option("host", "localhost").option("port", 9999).load() //with this, spark is making connection with the data coming at 9999 from netcat. this will give us a dataframe.
lines.writeStream.outputMode("complete").option("checkpoint", "file:///home/ubuntu/checkdir").format("console").start()
```
<br>
Tab3: start hadoop, master, slaves.
<br>

Use 2 terminals
 
 
NOTE: Start the netcat server first and then the streaming application
 
Terminal 1 : For netcat server
 
      nc -lk 9999 <hit enter>
<enter senetence  hit enter to publish it on 9999 port>
 
 
NOTE: To stop server , press CTRL +C
 
 
 
Terminal 2 : For pyspark shell
 
 
lines = spark.readStream.format("socket").option("host", "localhost").option("port", 9999).load()
lines.writeStream.outputMode("complete").option("checkpointLocation", "file:///home/ubuntu/checkdir").format("console").start()

Streaming doesn't support .show().
- Here also, we need:
   - StreamingDFReader: Helps construct the streaming df from streaming data sources (socket, file system, kafka)
   - StreamingDFWriter: Helps us write the streaming computation result onto the sink (console/file/kafka)
     ```
     df.writeStream.outputMode("update").option("checkpointLocation", "file:///home/vagrant/checkdir100").format("console").start() //format here means where we want to see the final result. outputMode here controls whcih part of the result table has to be written to external sink in each trigger. There are three output modes: update, append, complete.
     ```

### Questions:
1. Find total time spent on shopping site by each customer  

2. How many number of searches happened from each device type over every 30 seconds 

3. How many times "Jewellery"  products are searched over every 30 seconds, refresh the results every 10seconds 

#### Answer for Q1

In one tab: Write this: 
```
gedit streaming_webclicks.py
```

Then in streaming_webclicks.py: 
```
import time
import sys
def send_data_one_per_second():
	with open('/home/ubuntu/dataset/sourcedata/webclicksdata/goShopping_webclicks1.dat', 'r') as file:
		next(file)
		for line in file:
			print('{}' .format(line.strip()))
			sys.stdout.flush()
			time.sleep(0.5)
if __name__=="__main__":
	send_data_one_per_second()
```

Next, write this (make sure you aren't running nc -lk 9999 in another tab): 
```
python stream_webclicks.py | nc -lk 9999 
```
Then, 

```
from pyspark.sql.functions import sum, count, split, col, window, to_timestamp, concat_ws

spark = SparkSession.builder.appName("WebClicksStreaming").getOrCreate()


raw_stream = spark.readStream.format("socket").option("host", "localhost").option("port", 9999).load()

columns = ["date", "time", "hostIp", "csMethod", "customerIp", "url", "timeSpent", "redirectedFrom", "deviceType"]

df = raw_stream.withColumn("data", split(col("value"), "\t")).selectExpr("data[0] as date", "data[1] as time", "data[2] as hostIp","data[3] as csMethod", "data[4] as customerIp", "data[5] as url", "data[6] as timeSpent", "data[7] as redirectedFrom", "data[8] as deviceType").withColumn("timeSpent", col("timeSpent").cast("int"))

total_time_spent=df.groupBy("customerIp").agg(sum("timeSpent").alias("total_time_spent"))

query1=total_time_spent.writeStream.outputMode("complete").format("console").start()
```
