# SQL Database Fundamentals

## SQL Command Types

![SQL_Command_Types](https://github.com/danvuk567/SQL-Best-Practices/blob/main/images/SQL_Command_Types.jpg?raw=true)

### **What are DDL, DML, DCL, TCL, and DQL in SQL?**

**DDL:** Data Definition Language is used to create, modify, and drop the schema of database objects. DDL focuses on the structure of the database. Key DDL commands include: *CREATE*, *ALTER TABLE*, *DROP*, *TRUNCATE*, and *ADD COLUMN*.

**DML:** Data Manipulation language allow to change or manipulate the existing data of the tables. DML focuses on manipulating the data itself. Common DML commands include: *UPDATE*, *DELETE*, and *INSERT*.

**DCL:** Data Control Language allow Administrator of the database to manage the rights and permissions of the users in the database. DCL focuses on permissions and access control. The key DCL commands are: *GRANT* and *REVOKE*.

**TCL:** Transaction Control language is used to maintain the SQL operations within the database. It also allows the changes to be saved which are made by the DML commands. TCL focuses on transaction management. Common TCL commands include: *COMMIT*, *SET TRANSACTION*, *ROLLBACK*, and *SAVEPOINT*.

**DQL:** Data Query Language is used to query or retrieve data from the database. DQL specifically uses the *SELECT* statement.


## Database Object Types

**Tables:** are the fundamental storage objects in a database. They store the actual data in rows and columns. Each row represents a single record, and each column represents an attribute of the record.

**Views:** are virtual tables that contain a result set from a query. A view doesn't store data itself but displays data stored in other tables. It simplifies complex queries and can provide a security layer by limiting 
         user access to specific data.
         
**Indexes:** are created on one or more columns and allow for faster search and retrieval operations. Indexes improve query performance but can slow down write operations (INSERT, UPDATE, DELETE).

**Sequences:** are used to generate unique numeric values, often used to create primary key values automatically. They are commonly used in auto-incrementing columns like an ID column.

**Stored Procedures:** are precompiled SQL statements that can be executed as a unit. They allow for modular, reusable code and help centralize logic on the database server, making database operations more efficient.

**Triggers:** are automated actions that are executed in response to specific events (e.g., INSERT, UPDATE, DELETE) on a table or view. They are often used for enforcing business rules, maintaining data integrity, and 
              logging changes.

  The different types of triggers are:

  * **BEFORE Triggers:** is executed before the data modification operation (e.g., INSERT, UPDATE, DELETE) is carried out.

  * **AFTER Triggers:** is executed after the data modification operation has completed. The trigger is fired only after the operation has been successfully performed.
  
  * **INSTEAD OF Triggers:** is executed instead of the actual data modification operation. This allows you to override the default behavior (e.g., inserting data into a table) with custom logic.

  * **Event Based Triggers:** are based on the specific data modification event that causes them to fire. These events include INSERT, UPDATE, and DELETE.

  * **Scope Based Triggers:** are defined at the level of the row or the statement, depending on whether you want the trigger to execute for each individual row affected or just once per operation (statement-level).

**Constraints:** are rules applied to table columns to enforce data integrity. They ensure that data meets certain conditions before it is inserted or updated in the table.
  The different types of constraints are:

* **DEFAULT:** sets a default value for a column.
  
* **UNIQUE:** ensures all values are unique. It also creates a unique index.
  
* **NOT NULL:** prevents NULL values.
  
* **CHECK:** is used to limit the range of values that can be inserted into a column. It can be applied to one or more columns, and the condition must be true for each row in the table.
  
* **PRIMARY KEY:** uniquely identifies each record in a table and combines NOT NULL and UNIQUE constraints. It also creates a unique index.
  
* **FOREIGN KEY:** is used to enforce referential integrity between two tables. A foreign key links two tables referencing the primary key in another table. It also creates a non-unique index.

* **Schemas:** is a logical container that groups related database objects like tables, views, and procedures. Schemas help organize the database and manage permissions at a higher level.

* **Functions:** are similar to stored procedures but are used primarily to return a value.

* **Synonyms:** provide an alternative name for an existing database object and create a level of abstraction.

 ## Normalization

 Normalization in database design is the process of organizing the columns and tables of a relational database to minimize redundancy and dependency. Normalization, splits the big table into multiple sub tables and ensure that database integrity constraints are intact with their relationship with each other. Denormalization (the process of introducing redundancy) may be considered when performance optimization is necessary, particularly in read-heavy applications or when complex queries require multiple JOIN operations.




           

