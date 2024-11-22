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

* **LIMIT / OFFSET:** can be applied last and control how many rows are returned and from which point in the result set.

## Query Optimization Methods



