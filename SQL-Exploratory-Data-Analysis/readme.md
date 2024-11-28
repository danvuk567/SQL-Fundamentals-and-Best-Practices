* SQL Exploratory Data Analysis

** Table Indices and Constraints

Let's look an example of how we can find out what the indexes are on a table. We can use this query to find the **Primary Key** and any other indexes on the table called *Global_Index_Targets*. We see that the Primary Key *PK_Global_Index_Targets* is the primary eky on the *Date* and *Index_ID* columns. We also have a non-unique index *IDX_Global_Index_Targets* on the *Date* column.

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


