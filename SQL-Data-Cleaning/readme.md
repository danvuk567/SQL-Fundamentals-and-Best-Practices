# Data Cleaning Techniques using SQL

## Data Cleaning

![SQL_Data_Cleaning](https://github.com/danvuk567/SQL-Best-Practices/blob/main/images/SQL_Data_Cleaning.jpg?raw=true)

Data cleaning in SQL involves correcting errors, inconsistencies, and missing values. Here are a few methods that we will explore using SQL functions and techniques to improve data quality and accuracy.

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

Here we use DISTINCT() to remove duplicate values from our data set and can see that Ticker_Name = 'CHKP' appears once.

![SQL_Duplicates3.jpg](https://github.com/danvuk567/SQL-Best-Practices/blob/main/images/SQL_Duplicates3.jpg?raw=true)

To permanently remove duplicates from the table, we can use ROW_NUMBER() function to identify the duplicate rows and delete them.

![SQL_Duplicates4.jpg](https://github.com/danvuk567/SQL-Best-Practices/blob/main/images/SQL_Duplicates4.jpg?raw=true)

![SQL_Duplicates5.jpg](https://github.com/danvuk567/SQL-Best-Practices/blob/main/images/SQL_Duplicates5.jpg?raw=true)

**3. Standardize Text:** The **LOWER()**, **UPPER()** AND **TRIM()** functions can be used to ensure that text is consistent, which is important for comparisons, storage, and presentation.

The TRIM() function removes leading and trailing spaces. Using TRIM() WITH UPPER() OR LOWER() can also help identofy duplicates.

In this example. we can see that Ticker_Name = 'CHKP' has 2 for ROW_NUMBER() and that Ticker_Name = 'Chkp' exists which identifies the duplicate.

![SQL_Standardize_Text1.jpg](https://github.com/danvuk567/SQL-Best-Practices/blob/main/images/SQL_Standardize_Text1.jpg?raw=true)

![SQL_Standardize_Text2.jpg](https://github.com/danvuk567/SQL-Best-Practices/blob/main/images/SQL_Standardize_Text2.jpg?raw=true)

**4. Mispelled Text:** The **CHARINDEX**, **SUBSTRING**, and **DIFFERENCE** function can be used to correct text that may have been mispelled provided we also use a lookup table that contains the correct text. In this 
     example, we have the *category* dimension table containing product categories and the *orders* table. The category table has one or two words in the description so we can break the words up by space using CHARINDEX 
     and SUBSTRING or assign an empty string for the 2nd word where onse word exists. The same logic can be done for categories in the orders table. We search for the rows in the orders table where category does not 
     match description in the category table and we can check how close each word is in the orders table compared to the category table by using the DIFFERENCE function. Each word DIFFERENCE comparison indicates how 
     strong the match is so adding the *match_word1_strength* variable with the *match_word2_strength* gives us a total strength score as *match_word_strength*. We sort each product, category in the orders table by 
     match_word_strength. We notice that the Life Shaving Foam product with category *"Shing Crem"* has the highest *match_word_strength value = 7* and that the description is *"Shaving Cream"*. So we can assume that the 
     category was mispelt as "Shing Crem" and should be "Shaving Cream".

    WITH category_words AS
    (SELECT
      description,
      CASE 
       WHEN CHARINDEX(' ', description) > 0 THEN SUBSTRING(description, 1, CHARINDEX(' ', description) - 1)
       ELSE description
      END AS first_word,
      CASE 
       WHEN CHARINDEX(' ', description) > 0 THEN SUBSTRING(description, CHARINDEX(' ', description), LEN(description))
       ELSE ''
      END AS second_word
    FROM [Testing].[Test].category),
    order_words AS
    (SELECT
      order_date, 
      product,
      category,
      CASE 
       WHEN CHARINDEX(' ', category) > 0 THEN SUBSTRING(category, 1, CHARINDEX(' ', category) - 1)
       ELSE category
      END AS first_word,
      CASE 
       WHEN CHARINDEX(' ', category) > 0 THEN SUBSTRING(category, CHARINDEX(' ', category), LEN(category))
       ELSE ''
      END AS second_word,
      customer_name, 
      total_price
     FROM [Testing].[Test].orders),
     match_orders AS
     (SELECT
       o.order_date,
       o.product,
       o.category,
       c.description,
       o.first_word AS category_first_word,
       c.first_word AS description_first_word,
       o.second_word AS category_second_word,
       c.second_word AS description_second_word,
       DIFFERENCE(TRIM(o.first_word), TRIM(c.first_word)) AS match_word1_strength,
       DIFFERENCE(TRIM(o.second_word), TRIM(c.second_word)) AS match_word2_strength,
       DIFFERENCE(TRIM(o.first_word), TRIM(c.first_word)) + DIFFERENCE(TRIM(o.second_word), TRIM(c.second_word)) AS match_word_strength,
       o.customer_name, 
       o.total_price
      FROM order_words o, category_words c
      WHERE NOT EXISTS (SELECT 1 FROM [Testing].[Test].category c WHERE c.description = o.category))
      SELECT
       product,
       category,
       description,
       category_first_word,
       description_first_word,
       category_second_word,
       description_second_word,
       match_word1_strength,
       match_word2_strength,
       match_word_strength
      FROM match_orders
      ORDER BY product, category, match_word_strength DESC;

![SQL_Mispelled_Text1.jpg](https://github.com/danvuk567/SQL-Best-Practices/blob/main/images/SQL_Mispelled_Text1.jpg?raw=true)

Now let's refine the query and use the **MAX** function on *match_word_strength* to identify the rows that need to be corrected by replacing category by description as *new_category*. There were 2 cases which we have identified correctly using the query logic.

    WITH category_words AS
    (SELECT
      description,
      CASE 
       WHEN CHARINDEX(' ', description) > 0 THEN SUBSTRING(description, 1, CHARINDEX(' ', description) - 1)
       ELSE description
      END AS first_word,
      CASE 
       WHEN CHARINDEX(' ', description) > 0 THEN SUBSTRING(description, CHARINDEX(' ', description), LEN(description))
       ELSE ''
      END AS second_word
    FROM [Testing].[Test].category),
    order_words AS
    (SELECT
      order_date, 
      product,
      category,
      CASE 
       WHEN CHARINDEX(' ', category) > 0 THEN SUBSTRING(category, 1, CHARINDEX(' ', category) - 1)
       ELSE category
      END AS first_word,
      CASE 
       WHEN CHARINDEX(' ', category) > 0 THEN SUBSTRING(category, CHARINDEX(' ', category), LEN(category))
       ELSE ''
      END AS second_word,
      customer_name, 
      total_price
     FROM [Testing].[Test].orders),
     match_orders AS
     (SELECT
       o.order_date,
       o.product,
       o.category,
       c.description,
       o.first_word AS category_first_word,
       c.first_word AS description_first_word,
       o.second_word AS category_second_word,
       c.second_word AS description_second_word,
       DIFFERENCE(TRIM(o.first_word), TRIM(c.first_word)) AS match_word1_strength,
       DIFFERENCE(TRIM(o.second_word), TRIM(c.second_word)) AS match_word2_strength,
       DIFFERENCE(TRIM(o.first_word), TRIM(c.first_word)) + DIFFERENCE(TRIM(o.second_word), TRIM(c.second_word)) AS match_word_strength,
       o.customer_name, 
       o.total_price
      FROM order_words o, category_words c
      WHERE NOT EXISTS (SELECT 1 FROM [Testing].[Test].category c WHERE c.description = o.category))
      SELECT
       order_date,
       product,
       category,
       description AS new_category,
       customer_name, 
       total_price
      FROM match_orders
      WHERE match_word_strength IN (SELECT MAX(match_word_strength) FROM match_orders GROUP BY product, category);

![SQL_Mispelled_Text2.jpg](https://github.com/danvuk567/SQL-Best-Practices/blob/main/images/SQL_Mispelled_Text2.jpg?raw=true)

**5. Date Formatting:** The **FORMAT** function can be used to extract a string with a different date format from the order date. The **CAST** can be used to extract the time from the order date.

![SQL_Date_Formatting.jpg](https://github.com/danvuk567/SQL-Best-Practices/blob/main/images/SQL_Date_Formatting.jpg?raw=true)

**6. Remove Bad Characters:** The **TRANSLATE** function can be used to extract to replace a list of characters with a space for each. We can then use **REPLACE** to put one space where there are two spaces and then **RTRIM** and **LTRIM** to trim any space in 1st position or last position. The new_product columns has the bad characters removed for the products that were found with this criteria.

![Remove_Bad_Characaters.jpg](https://github.com/danvuk567/SQL-Best-Practices/blob/main/images/Remove_Bad_Characaters.jpg?raw=true)

**7. Outlier Validation:** Certain numberical values may not make sense and are considered outliers. In this example, we look for any *total_price* that is negative, $0 or greater than or equal to $500.

![SQL_Outliers.jpg](https://github.com/danvuk567/SQL-Best-Practices/blob/main/images/SQL_Outliers.jpg?raw=true)

**8. Combine Text:** The **CONCAT** function can be used to combine text. Here is an example of combining the *first_name* and *last_name* as *name* column.

![SQL_Combine_Text.jpg](https://github.com/danvuk567/SQL-Best-Practices/blob/main/images/SQL_Combine_Text.jpg?raw=true)

**9. Text Validation:** The **PATINDEX** function can be used to do pattern matching and validate if certain text requires the inclusion of certain characters. In this example, we look for emails that is missing the *@* character which would be invalid.

![SQL_Text_Validation.jpg](https://github.com/danvuk567/SQL-Best-Practices/blob/main/images/SQL_Text_Validation.jpg?raw=true)
