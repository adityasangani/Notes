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
