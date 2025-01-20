## Types of Data
- Quantitative Data
- Qualitative Data: Descriptive Data

- Structured data
- Semi-structured Data: json files
- Unstructured Data: raw data, word file

- Primary Data: Directly from the first source.
- Secondary Data: Getting data from other sources, third party data.
- Meta Data: Data about data.
Data Analytics: Explore data and take some meaningful business decisions.
Data Aggregation: Collecting/Gathering data.

Why we don't use 
To access data faster
To reduce duplicity of data

ACID: Atomicity, Consistency, Isolation, Durability.
Isolation: In case of multiple transactions, one transaction should not affect another.

Employee, Department, Department 
![image](https://github.com/user-attachments/assets/cae6786d-6ff8-47ca-bdb4-e8d00f5714d7)
![image](https://github.com/user-attachments/assets/871315a0-47e8-4145-9f17-bab66f90b6ec)
![image](https://github.com/user-attachments/assets/5d484473-8284-4b0a-8417-fe6898948794)
![image](https://github.com/user-attachments/assets/f86b92cc-c1dc-4d2f-ad21-7d1295fe51f5)
![image](https://github.com/user-attachments/assets/d31d993a-a92d-4a95-bc20-1c9f12ce2d6e)
![image](https://github.com/user-attachments/assets/66060d3c-5c5f-4178-9bd2-e2791be6377e)

To add a column after a particular column: 
alter table employee add column DateOfBirth date after employeename;

### Insert
![image](https://github.com/user-attachments/assets/4ae8f708-309a-4208-87a3-d6be1ca8adba)

Truncate : Doesn't remove the table structure. Once you do truncate, you cannot do rollback.
Drop: Removes the entire table including the data structure. You cannot rollback.
![image](https://github.com/user-attachments/assets/c23368e6-7fe5-45e9-9f82-2128f662554f)

### DML
![image](https://github.com/user-attachments/assets/920e4c5e-1bf9-4a18-9c0c-96e4a28134c4)
UPDATE: update EMPLOYEE set dept = 'Finance';
Used to update the column values in the table.
![image](https://github.com/user-attachments/assets/aca31035-409b-4f45-ac69-1102be3ea091)

#### DELETE Statement
![image](https://github.com/user-attachments/assets/5d0fbbe1-a6d5-436a-b343-675191e49510)

![image](https://github.com/user-attachments/assets/570b4903-e235-4955-93e5-4433bac45db2)
![image](https://github.com/user-attachments/assets/69ab6305-f64b-4e72-935a-abbf0b7697d4)
![image](https://github.com/user-attachments/assets/20a851c8-0541-401f-9a5a-8202d1ee79b9)

![image](https://github.com/user-attachments/assets/5d28a48d-0785-400f-b457-41d9cca3e7e3)
![image](https://github.com/user-attachments/assets/d88d7f05-33e5-46ca-b8a6-08cb3dbf8083)

![image](https://github.com/user-attachments/assets/6ad06506-ebdb-4765-a64b-78e894acb964)

LEFT JOIN and LEFT OUTER JOIN are the same in SQL.

The term OUTER is optional in the case of LEFT JOIN. Both refer to the same type of join, which returns all rows from the left table and the matching rows from the right table. If there is no match, the result will contain NULL values for columns from the right table.

## Data Warehousing Concepts
![image](https://github.com/user-attachments/assets/3ffcaf3d-770c-4472-8292-d9ecd1130587)
![image](https://github.com/user-attachments/assets/2f9c64a4-9383-4f26-9ba8-9a8c85f5a4f8)
Extraction: Collecting all the data
Transformation: Some business logic which converts unstructured data into structured data.  
Loading: Saving the transformed data into the target data warehouse or database.

There are different ways of storing the data.
1. Subject Oriented
![image](https://github.com/user-attachments/assets/428c5c3f-5eb2-40e9-84dc-b36f68ccf01c)
2. Time Variant
![image](https://github.com/user-attachments/assets/ce922e73-68dd-4edf-8713-d55ab765d2c2)
Here, the data keeps on changing with time.
3. Integrated
![image](https://github.com/user-attachments/assets/a28ffedf-fc9c-4962-8fcc-605ae8a500ff)
4. Non-Volatile
The stored data is never removed. The history is retained.
![image](https://github.com/user-attachments/assets/aea89fb9-3b4a-47c6-a5d0-92c6483e8e04)

![image](https://github.com/user-attachments/assets/826c0b99-93cc-4fcd-9af8-6b3c9e8c0c52)
![image](https://github.com/user-attachments/assets/d39494cd-db3c-4dfe-bc34-3ac24d6f19d2)

### DW Schema
![image](https://github.com/user-attachments/assets/7e808d35-a7b6-4044-bfb1-5bdade127d18)
![image](https://github.com/user-attachments/assets/3c091d5d-8f11-406a-82d4-1566b057117a)
![image](https://github.com/user-attachments/assets/817e4692-f883-413a-b3ae-3331a16b08aa)

### DW Objects
![image](https://github.com/user-attachments/assets/5b9e0644-0939-42b8-942d-dc50d2148c96)
![image](https://github.com/user-attachments/assets/0624b41d-89f2-4b33-90f2-503302717f26)
![image](https://github.com/user-attachments/assets/2aa25c88-fd8b-4a5e-b4f4-7a54002ce1cc)
![image](https://github.com/user-attachments/assets/31f875f5-dce3-4f13-bd25-e2c18317c22c)
![image](https://github.com/user-attachments/assets/102a90b6-14d1-44c2-a5de-e7904027cc89)
![image](https://github.com/user-attachments/assets/95ed60ed-778d-4947-b313-947a4e7aa845)
![image](https://github.com/user-attachments/assets/a171a993-9536-4e49-9850-03aa4c80dff8)
![image](https://github.com/user-attachments/assets/7e0b1580-0e52-49c9-841c-04d3dd422a6d)
![image](https://github.com/user-attachments/assets/d86a1a71-5fc7-4313-a1ea-e7c4f162db5a)

### Extract
![image](https://github.com/user-attachments/assets/d58189de-15d7-4753-a7e5-d0618f09e734)
![image](https://github.com/user-attachments/assets/a401bad6-8c54-4b1c-ac5d-96a51ff9eff3)

### Transform


### Load
![image](https://github.com/user-attachments/assets/0272b8c9-35a0-4668-a28e-489b0a765785)

### ELT
![image](https://github.com/user-attachments/assets/a8eac859-1970-4467-adb6-657fe1b6db2d)
![image](https://github.com/user-attachments/assets/61fff549-582e-4ac9-92d4-36f5c62c316d)
![image](https://github.com/user-attachments/assets/4b0d13ad-5e5d-4481-8519-b5861b779f15)

### Big Data
![image](https://github.com/user-attachments/assets/b18012e9-ca80-4626-86cf-9b0130ac69b2)

## Different Uses of Databases
Two common types of databases:
### Online Transaction Processing (OLTP) Database
Used for applications that handle immediate user transactions, such as entering data or retreiving data from a screen or report. Multiple users can access this database simultaneously without having problems of data contention or integrity. 

### Online Analytical Processing (OLAP) Database
Used for applications that handle massive data analysis for report generation and forecasting. 
Data is entered into an OLAP massively from various systems in batch mode.
Here, users are not looking for specific set of data but rather patterns or behaviour of data.

### ACID Properties of a Database
#### A = Atomicity
- This property guarantees that an action is only executed on the database when all of its related actions are successfully executed.
- Transaction: It is a set of interrelated activities in a database, where all must be completed for it to be successful.

#### C = Consistency
- This property guarantees that data residing inside a database is always consistent with the rules that have been set for the database.
- If an action on the database is not following the rules, it won't be allowed. 

#### I = Isolation
- This property guarantees that while you're updating a piece of data, everyone else trying to access(i.e read) the same data will see the original version of the data, and will not be able to see changes in an intermediate state.
- To implement isolation, the database can do locking.

#### D = Durability
- This property guarantees that once a user is notified of success in his action on the database, the completed action will persist. Even if the power supply of the building turns down, the database should be able to recover the updated data once the power returns.

### Database System Components
1. CPU
2. Memory: It is the intermediate stage between 'data in the disk' and 'data being processed in the CPU'.
