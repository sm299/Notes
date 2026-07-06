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



## NoSQL (Not Only SQL)

NoSQL databases are non-relational databases designed to handle large volumes of unstructured or semi-structured data. They provide flexible schemas and are well-suited for modern, scalable applications.<br>

Key Features:<br>

Flexible or schema-less data model.<br>
Can store data as documents, key-value pairs, graphs, or columns.<br>
Easily scalable for large datasets.<br>
High performance for distributed applications.<br>

Examples of NoSQL databases:<br>

MongoDB (Document)<br>
Redis (Key-Value)<br>
Cassandra (Column)<br>
Neo4j (Graph)<br>