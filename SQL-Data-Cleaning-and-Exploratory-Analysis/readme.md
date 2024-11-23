# Data Cleaning Techniques and Exploratory Data Analysis using SQL

## Data Cleaning

![SQL_Data_Cleaning](https://github.com/danvuk567/SQL-Best-Practices/blob/main/images/SQL_Data_Cleaning.jpg?raw=true)

Data cleaning in SQL involves correcting errors, inconsistancies, and missing values. The following topics will explore SQL functions and techniques to improve data quality and accuracy.

**1. Handling missing values:** The **COALESCE()**, **IFNULL()** and **IFNULL()** function can be used to replace missing or NULL values in a table.

Let's look at an example in an SQL Server database where we have NULL values for a column called *Close_EMA_200*.

![SQL_COALESCE1.jpg](https://github.com/danvuk567/SQL-Best-Practices/blob/main/images/SQL_COALESCE1.jpg?raw=true)

We can use the COALESCE function that returns the first non-NULL value from a list of arguments where we replace NULL with the value 0.

![SQL_COALESCE2.jpg](https://github.com/danvuk567/SQL-Best-Practices/blob/main/images/SQL_COALESCE2.jpg?raw=true)

Let's update one of the values to 0 and then show the results.

![SQL_COALESCE3.jpg](https://github.com/danvuk567/SQL-Best-Practices/blob/main/images/SQL_COALESCE3.jpg?raw=true)

![SQL_COALESCE4.jpg](https://github.com/danvuk567/SQL-Best-Practices/blob/main/images/SQL_COALESCE4.jpg?raw=true)

**2. Removing Duplicates:** The **DISTINCT()** function, **COUNT()**, and **ROW_NUMBER()** function can be used to identify duplicates.

Here is an example where we identify duplicate *Ticker_Name* column values using COUNT(). We can see that Ticker_Name = 'CHKP' has duplicates.

![SQL_Duplicates1.jpg](https://github.com/danvuk567/SQL-Best-Practices/blob/main/images/SQL_Duplicates1.jpg?raw=true)

![SQL_Duplicates2.jpg](https://github.com/danvuk567/SQL-Best-Practices/blob/main/images/SQL_Duplicates2.jpg?raw=true)

Here we use DISTINCT() to remove duplicate values from our data set.

![SQL_Duplicates3.jpg](https://github.com/danvuk567/SQL-Best-Practices/blob/main/images/SQL_Duplicates3.jpg?raw=true)

To permanantly remove duplicates from the table, we can use ROW_NUMBER() function to identify the duplicate rows and delete them.

![SQL_Duplicates4.jpg](https://github.com/danvuk567/SQL-Best-Practices/blob/main/images/SQL_Duplicates4.jpg?raw=true)

![SQL_Duplicates5.jpg](https://github.com/danvuk567/SQL-Best-Practices/blob/main/images/SQL_Duplicates5.jpg?raw=true)


## Exploratory Data Analysis

![SQL_EDA](https://github.com/danvuk567/SQL-Best-Practices/blob/main/images/SQL_EDA.jpg?raw=true)
