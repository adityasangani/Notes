Hive is a data lake on top of hadoop. 

start-dfs.sh
start-yarn.sh

In another tab,
Type “hive”
create database if not exists goshopping;
Use goshopping;

create table iplookup(ip string, country string, state string, city string, lat string, long string) row format delimited fields terminated by ‘,’ lines terminated by ‘\n’ stored as textfile
Two types of delimiter 
Field terminater 
Lines terminator 

/user/hive/warehouse is the default 


Create database if not exists test1
Comment ‘holds all the test tables’
location ‘user/ubuntu/test1’

# Partitioning
Two types of partitioning: 
## Static Partitioning (By Default)
Table creation syntax is same for static and dynamic partitioning.
```
create table user_data
(
sno int,
usr_name string
) partitioned by (city string);
```
We cannot insert data into a partition table via load command (load data inpath waala)!
We can only use 'insert into' or 'insert overwrite'
```
insert into table user_data partition (city='chennai') select sno, usr_name from user_data_no_partition where city='chennai';
```
To see the partitions:
```
show partitions user_data;
```

## Dynamic Partitioning
```
create table user_data
(
sno int,
usr_name string
) partitioned by (city string);
```
By default, hive disables inserting data in dynamic partitioning way. So to enable it:
```
set hive.exec.dynamic.partition.mode=nonstrict;
```

Now to insert dynamically,
```
insert into table user_data_dynamic partition **(city)** select sno, usr_name, **city** from user_data_no_partition where city='chennai';
```

