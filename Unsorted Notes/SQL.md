DBMS programming language that handles interaction between user and database.
Designed for relational databases - which are organized by tables of rows and columns
## Commands
- CREATE {data type} {name}; - creates a new instance of a data type
	- for tables, specify what each columns represents
- SHOW {data type}; - shows all present instances of a data type.
- USE {database_name} - use a specified database
- DROP {data type} {name} - removes the named data type, such as a database or table
- DESCRIBE/DESC {name} - give info about a specified table 
- ALTER TABLE {name} - add columns to an existing table.
- INSERT INTO - insert a new entry/row into the table. Specify each field
- SELECT - select specified columns from a table. Use * to grab all columns.
- UPDATE - updates an existing entry in a table
- DELETE - delete a record in a table.
## Clauses
Allows you to specify which resources to grab.
- FROM - specify table to grab
- WHERE - specify which records to grab
- DISTINCT - only list unique values
- GROUP BY - aggregates data from multiple records, groups results in columns
- ORDER BY - sort data in ascending or descending orders (ASC/DESC)
- HAVING - filters results that match a specific criteria
## Logical Operators
Create different T/F clauses to find matching results.
- LIKE - filters by a specified pattern
- AND/OR/NOT - typical boolean operators
- BETWEEN - checks if a value is within a certain range
- Comparison operators: =, !=, >, <, >=, <=

## Other Functions
Manipulate data and streamline queries and operations
- CONCAT() - add two or more strings together, useful to combine texts from different columns
- GROUP_CONCAT() - concatenate strings from multiple rows into one field.
- SUBSTRING() - retrieves a substring with specified indicies.
- LENGTH()
- Aggregate Functions
	- COUNT() - returns number of records
	- SUM(), MAX(), MIN() 

## SQL Injection
A vulnerability where user inputs are fed into SQL queries, but inputs are not properly handled.
- sqlmap helps to automate the process of discovering and using SQL injection