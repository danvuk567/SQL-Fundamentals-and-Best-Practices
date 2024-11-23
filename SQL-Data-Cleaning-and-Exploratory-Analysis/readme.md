# Data Cleaning Techniques and Exploratory Data Analysis using SQL

## Data Cleaning

![SQL_Data_Cleaning](https://github.com/danvuk567/SQL-Best-Practices/blob/main/images/SQL_Data_Cleaning.jpg?raw=true)

Data cleaning in SQL involves correcting errors, inconsistancies, and missing values. The following topics will explore SQL functions and techniques to improve data quality and accuracy.

**1. Handling missing values:** The COALESCE(), IFNULL() and ISNULL() function can be used to replace missing or NULL values in a table.

Let's look at an example in an SQL Server database where we have NULL values.



We can use the COALESCE function that returns the first non-NULL value from a list of arguments where we replace NULL with the value 0.

Let's update one of the values to 0 and then show the results.

## Exploratory Data Analysis

![SQL_EDA](https://github.com/danvuk567/SQL-Best-Practices/blob/main/images/SQL_EDA.jpg?raw=true)
