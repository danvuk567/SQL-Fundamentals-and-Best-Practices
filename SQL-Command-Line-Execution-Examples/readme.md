# SQL Command Line Execution Examples

![SQL_Terminal_Commands](https://github.com/danvuk567/SQL-Best-Practices/blob/main/images/SQL_Terminal_Commands.jpg?raw=true)

What are a few different ways we can run SQL commands at the commande line?

## **Windows Command Terminal** ##

For Windows, SQL commands can be run using a SQL application via the **cmd terminal** or in **.bat batch file**. Normally, *-S* is used for the server name parameter along with *-d* for database, *-u* for username and *-p* for password.

In this example using SQL server, we can run a SQL query command for our local machine using the *-Q* parameter and *-v* to pass the variable dept_name to the query:

    sqlcmd -S localhost -d financial_securities -U user_name -P pass_word -v ticker=’MSFT’ -Q “SELECT * FROM equities WHERE ticker = '$(ticker)';"

We can also run a SQL script file using the *-i* parameter:

    sqlcmd -S localhost -U your_username -P your_password -v ticker=’MSFT’ -i "C:\path\dept_sales_query.sql"

Or we can simply run SQL commnds in interactive mode by calling the SQL application at the prompt:

    sqlcmd
    1> SELECT * FROM Financial_Securities.Equities.Equities;
    2> GO
    ...
    1> EXIT

## **Windows Powershell** ##

Another method is using PowerShell which has advanced scripting abilities to interact with databases and where you can define variables, loops, conditional statements, and even error handling directly within the scripts.

In this example, we can run a query with a Powershell script in the **PowerShell console** or via a **Powershell .ps1 file** and export the results to a csv file:

    Import-Module SqlServer

    $server = "localhost" 
    $database = "financial_securities" 
    $username = "user_name"
    $password = "pass_word"
    $query = "SELECT * FROM equities"
    $export_path = “C:\path\output.csv”

    $result = Invoke-Sqlcmd -ServerInstance $server -Database $database -Query $query -U $username -P $password
    
    if ($result) { 
        $result | Export-Csv -Path $export_path -NoTypeInformation 
        Write-Host "Query results have been exported to $export_path"
    }
    else {
        Write-Host "No data returned from query."
    }

## **Linux Bash Shell** ##

In Linux, we can run SQL commands in the **bash shell terminal** using the SQL application such as SQL Server:

    sqlcmd -S localhost -d financial_securities -U user_name -P pass_word -Q "SELECT * FROM equities"

Or we can run SQL commands in a **bash script .sh file** in the terminal as long as it had executable permissions (chmod +x run_sql_query.sh). This script will export the results to csv:

    #!/bin/bash 
    
    SERVER="localhost" 
    DATABASE="Financial_Securities" 
    USER="user_name" 
    PASSWORD=" pass_word" 
    QUERY="SELECT * FROM equities" 
    EXPORT_PATH="/path/output.csv" 
    
    sqlcmd -S $SERVER -d $DATABASE -U $USER -P $PASSWORD -Q "$QUERY" -o $EXPORT_PATH -s "," -W 

    if [ $? -eq 0 ]; then 
        echo "Query results have been exported to $EXPORT_PATH" 
    else
        echo "There was an error executing the query." 
    fi
<br/><br/>

:arrow_right: **Next:** [SQL Table Structure Exploration and Data Analysis](https://github.com/danvuk567/SQL-Fundamentals-and-Best-Practices/tree/main/SQL-Table-Structure-Exploration-and-Data-Analysis)


