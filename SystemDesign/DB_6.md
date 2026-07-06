# DataBase->

## SQL (Structured Query Language)

SQL is a language used to interact with relational databases. It is used to create, read, update, and delete data (CRUD operations).<br>

*Key Features:* <br>

Data is stored in tables (rows and columns).<br>
Uses a fixed schema (structure is predefined).<br>
Supports relationships using primary keys and foreign keys.<br>
Follows ACID properties for reliable transactions.<br>

Examples of SQL databases:<br>

MySQL<br>
PostgreSQL<br>
Oracle Database<br>
Microsoft SQL Server<br>

### SQL Constraints->
1. Unique <br>
2. Not Null <br>
3. Primary Key <br>
4. Check (Used for Validation) <br>
5. Foreign Key <br>
6. Default (Null can't be the value) -> Suppose for Telusko every user is assigned the default role as learners. <br>

### JOINS->
1. One to Many -> Mapping between User and Blogs. <br>
![alt text](image-2.png) <br>
2. Many to One -> Mapping between Blogs to User. <br>
3. Many to Many -> Mapping between Courses and Students. <br>
   ![alt text](image-4.png) <br>

4. One to One -> Mapping between Account to Locker <br>


*It is Hard to scale in SQL DB, for Vertical Scaling it's manageable but after a point we have to go for Horizontal Scaling, which means having different servers across different places. So in that case, we have to maintain relations among entities in all of the databases spreaded among the servers. In these scanarios, NoSQL dbs are much easier to handle.*

*SQL is used when it comes to Payments,Transactions,Consistency.*
*NoSQL is used when it comes to Scale,Speed,Unstructured Data.*


## NoSQL (Not Only SQL)

NoSQL databases are non-relational databases designed to handle large volumes of unstructured or semi-structured data. They provide flexible schemas and are well-suited for modern, scalable applications.<br>

Key Features:<br>

Flexible or schema-less data model.<br>
Can store data as documents, key-value pairs, graphs, or columns, which can be different types, not bounded in same structure like SQL DBs.<br>
Easily scalable for large datasets both vertically and horizontally.<br>
High performance for distributed applications.<br>

Examples of NoSQL databases:<br>

MongoDB (Document)<br>
Redis (Key-Value)<br>
Cassandra (Column)<br>
Neo4j (Graph)<br>

### Types ->
#### Key-Value Pair ->
1. The main criteria is the Key should be unique.
2. Value can be anything like JSON, String, BLOB, ByteArray, Objects or combintions of these.


#### Columnal DB ->
Suppose the below is the table <br>
```
id | name     | marks
1  | Shreya G | 95
2  | Baishali | 90
3  | Shreya M | 55
```
In this case if we need to have avg marks, then for SQL db, it will read the data from left to right on a whole, even if we need only one column. <br>
But for Columnal DB, we can read data column-wise and we can link the data using common ids. So the common IDs will be there whenever we are reading column-wise and it will be easier and faster through our unique identifier i.e. ID in this case. <br>
Ex: Google Big Query <br>
Amazon RedShift <br>
SnowFlake <br>


#### Graph DB ->
It has different nodes and are joined by the threads which are called Edge. So every Edge basically represent a common relationship between two nodes. Nodes are entities. Here both Nodes and Edges can have properties. Suppose we are creating any node which is Student and we have another node Course and the relation(edge) is enrolled. Now this relation/edge can have properties like year-Of-Passing, Passing-Marks etc. This kind of DB is mainly used by Data Scientist to find patterns between different scenarios. <br>
Ex: Gremlin <br>
SparkQL <br>
Cypher <br>

#### Document DB ->
It is Schema-less. So Document DB also don't have to follow any relationship. Highly used for logging and profile section and contents.<br>
Ex: CouchDB, MongoDB <br>
