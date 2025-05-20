# SQL Table Structure Exploration and Data Analysis

![SQL_Table_Structure_Data.jpg](https://github.com/danvuk567/SQL-Best-Practices/blob/main/images/SQL_Table_Structure_Data.jpg?raw=true)

## Table Indices and Constraints

Let's look an example of how we can find out what the indexes are on a table. We can use the system table **sys.indexes** and **sys.index_columns** in this query to find the **Primary Key** and any other indexes on the table called *Global_Index_Targets*. We see that *PK_Global_Index_Targets* is the Primary Key on the *Date* and *Index_ID* columns. We also have a non-unique index *IDX_Global_Index_Targets* on the *Date* column.

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

Let's look at what the table structure looks like. Using the **INFORMATION_SCHEMA.COLUMNS** table, we can count he number of columns and then explore what type of data is in those columns. The following queries show that there are 5 columns with date, int and real types and no string types. *Max_Date* and *Target_Weight* can have null values.

        SELECT 
          COUNT(COLUMN_NAME) AS "Number of Columns"
        FROM INFORMATION_SCHEMA.COLUMNS
        WHERE table_name = 'Global_Index_Targets'
          AND table_schema = 'dbo';

        SELECT 
          COLUMN_NAME, 
          DATA_TYPE, 
          CHARACTER_MAXIMUM_LENGTH,
          IS_NULLABLE
        FROM INFORMATION_SCHEMA.COLUMNS
        WHERE table_name = 'Global_Index_Targets'
        AND table_schema = 'dbo';          
          
![SQL_Table_Structure.jpg](https://github.com/danvuk567/SQL-Best-Practices/blob/main/images/SQL_Table_Structure.jpg?raw=true)

## Table Data Exploration

Let's look at the first 5 rows using the **TOP** function and then use **COUNT**, **DISTINCT**, **MIN**, **MAX**, and **AVG** to explore what the data looks like. There are *44* rows in the *Global_Index_Targets* table, 2 distinct *Min_Date* values, and average of *0.194* for *Target_Weight* which is closer to the minimum value of *0.0286* than the maximum value of *0.194*. This may indicate that the higher values closer to the max are not as common. There are also no missing values in the *Target_Weight* column.

        SELECT TOP 5 *
        FROM dbo.Global_Index_Targets;

        SELECT 
         COUNT(*) AS "Number of Rows",
         COUNT(DISTINCT Min_Date) AS "Number of Distinct Min Dates",
         ROUND(MIN(Target_Weight), 4) AS "Min Target Weight",
         ROUND(AVG(Target_Weight), 4) AS "Avg Target Weight",
         ROUND(MAX(Target_Weight), 4) AS "Max Target Weight",
         COUNT(CASE WHEN Target_Weight IS NULL THEN 1 END) AS "Number of Target Weight Missing Values"
        FROM dbo.Global_Index_Targets;

![SQL_Table_Data_Exploration.jpg](https://github.com/danvuk567/SQL-Best-Practices/blob/main/images/SQL_Table_Data_Exploration.jpg?raw=true)<br/><br/>

:arrow_right: **Next:** [SQL Data Cleaning](https://github.com/danvuk567/SQL-Fundamentals-and-Best-Practices/tree/main/SQL-Data-Cleaning)

