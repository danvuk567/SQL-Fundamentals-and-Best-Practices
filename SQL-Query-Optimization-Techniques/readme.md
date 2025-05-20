# SQL Query Optimization Techniques

![Optimize-SQL-Queries](https://github.com/danvuk567/SQL-Best-Practices/blob/main/images/Optimize-SQL-Queries.jpg?raw=true)

## Query Execution Order

The order in which different clauses and operations are executed during the query process is often different from the order in which they appear in the query itself.
They are typically exececuted in this order:

* **FROM:** is executed first because it determines the tables or views from which data will be retrieved.

* **JOIN:** operations are executed after the FROM clause and determine how rows from different tables should be combined based on a condition (ON, USING).

* **WHERE:** filters rows based on a specified condition and removes rows from the result set that do not meet the condition defined in the WHERE clause.

* **GROUP BY:** includes an aggregate function (SUM, AVG, COUNT, etc.) and groups the rows into distinct sets based on the values of the specified columns.

* **HAVING:** is similar to the WHERE clause, but it is applied to the groups formed by the GROUP BY clause and filters the grouped rows after the aggregation is done.

* **SELECT:** specifies which columns or expressions should be included in the final result set.

* **DISTINCT:** can be applied after the SELECT clause to eliminate duplicate rows from the result set.

* **ORDER BY:** sorts the result set based on one or more columns and is one of the final steps before returning the result set.

* **LIMIT / OFFSET:** can be applied last and control how many rows are returned and from which point in the result set. **TOP** is used in SQL Server.

## Query Optimization Methods

**1.** When joining multiple tables using the **JOIN** clause, **join the smaller tables first before progressively joining larger tables**. When you join the smallest table first, the database engine can cache the smaller dataset in memory more easily, since it requires less memory to store. This can speed up processing for the subsequent joins, especially when the data doesn't fit entirely into memory and the database has to rely on disk I/O. If you start with the smallest table, you also reduce the number of rows that need to be matched in the next join. 

**2.** Create **Indexes** on columns that are frequently used in the WHERE, JOIN, or ORDER BY clauses. This way, you locate the rows that match the condition, rather than scanning the entire table which slows down performance on larger tables.

**3.** Use the **WHERE** clause when possible before applying other operations like JOIN or GROUP BY to reduce the data being pulled. This is especially true when HAVING clause is also being used in order to reduce the rows needed to be aggregated.
  
**4.** Use **HAVING** to filter aggregated data. Don't use it for non-aggrageted data, instead use the WHERE clause.
   
**5.** Avoid **Nested Loops** where a query contains one query inside another, where the inner query is executed multiple times—often once for each row returned by the outer query. This can significantly degrade 
       performance, especially when working with large datasets. Instead use JOINS or Common Table Expresssions (CTEs) using the **WITH** clause.

**6.** Only use the columns you need in the **SELECT** clause to reduce data transfer to speed up query execution.
   
**7.** Use **EXPLAIN PLAN** when performance is a concern. It is a command that shows the execution plan of an SQL query, helping you understand how the database processes the query and identifying areas for optimization.<br/><br/>

:arrow_right: **Back to:** [Home Page](https://github.com/danvuk567/SQL-Fundamentals-and-Best-Practices)





