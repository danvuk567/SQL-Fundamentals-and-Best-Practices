# SQL Exploratory Table Structure and Data Analysis

## Table Indices and Constraints

Let's look an example of how we can find out what the indexes are on a table. We can use the system tablea **sys.indexes** and **sys.index_columns** in this query to find the **Primary Key** and any other indexes on the table called *Global_Index_Targets*. We see that *PK_Global_Index_Targets* is the Primary Key on the *Date* and *Index_ID* columns. We also have a non-unique index *IDX_Global_Index_Targets* on the *Date* column.

    SELECT 
     i.name AS index_name,
     i.type_desc AS index_type,
     i.is_unique,
     c.name AS column_name,
     ic.key_ordinal AS column_position
    FROM sys.indexes i
    JOIN sys.index_columns ic 
     ON i.object_id = ic.object_id AND i.index_id = ic.index_id
    JOIN sys.columns c 
     ON ic.object_id = c.object_id AND ic.column_id = c.column_id
    WHERE i.object_id = OBJECT_ID('dbo.Global_Index_Targets');

![SQL_Table_Indexes.jpg](https://github.com/danvuk567/SQL-Best-Practices/blob/main/images/SQL_Table_Indexes.jpg?raw=true)

For any **Foreign Key** constraints that may exist on a table, this complex query that references **sys.foreign_keys**, **sys.foreign_key_columns**, **sys.tables**, and **sys.columns** can retrieve relevant information. In this example, we see that the Foreign Key *FK_Global_Index_Targ_Index_ID* on the parent table *Global_Index_Targets* references the table *Global_Indices* on the column *Index_ID* in both tables.

        SELECT 
          fk.name AS foreign_key_name,
          tp.name AS parent_table,
          ref.name AS referenced_table,
          c1.name AS parent_column,
          c2.name AS referenced_column,
          fkc.constraint_column_id AS column_position
        FROM sys.foreign_keys fk
        JOIN sys.foreign_key_columns fkc 
         ON fk.object_id = fkc.constraint_object_id
        JOIN sys.tables tp 
         ON fkc.parent_object_id = tp.object_id
        JOIN sys.tables ref 
         ON fkc.referenced_object_id = ref.object_id
        JOIN sys.columns c1 
         ON fkc.parent_column_id = c1.column_id AND fkc.parent_object_id = c1.object_id
        JOIN sys.columns c2 
         ON fkc.referenced_column_id = c2.column_id AND fkc.referenced_object_id = c2.object_id
        WHERE tp.name = 'Global_Index_Targets'
         AND tp.schema_id = SCHEMA_ID('dbo');
         
![SQL_Table_Foreign_Key.jpg](https://github.com/danvuk567/SQL-Best-Practices/blob/main/images/SQL_Table_Foreign_Key.jpg?raw=true)

## Table Structure

Let's look at what the table structure looks like. Using the **INFORMATION_SCHEMA.COLUMNS** table, we can count he number of columns and then explore what type of data is in those columns.

        SELECT COUNT(COLUMN_NAME) AS "Number of Columns"
        FROM INFORMATION_SCHEMA.COLUMNS
        WHERE table_name = 'Global_Index_Targets'
          AND table_schema = 'dbo';
