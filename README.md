# MySQL Cheat Sheet: Basics & Intermediate
>This cheat sheet offers clear MySQL queries and commands with easy examples. It covers key MySQL topics to help you learn and reference quickly.
>
## Table of Contents
- [Installation](#installation)
- [Starting MySQL](#starting-mysql)
- [Databases](#databases)
- [Tables](#tables)
- [Insert Rows](#insert-rows)
- [Update & Delete](#update--delete)
- [Select](#select)
- [Autocommit, Commit, Rollback](#autocommit-commit-rollback)
- [Current Date & Time Functions](#current-date--time-functions)
- [Constraints](#constraints)
- [Joins](#joins)
- [Functions](#functions)
- [Logical Operators](#logical-operators)
- [Wildcards](#wildcards)
- [Order By](#order-by)
- [Limit](#limit)
- [DISTINCT](#distinct)
- [On Delete (Cascade)](#on-delete-cascade)
- [Group By & Rollup (HAVING)](#group-by--rollup-having)
- [Unions](#unions)
- [Self Joins](#self-joins)
- [Subqueries](#subqueries)
- [Views](#views)
- [Indexes](#indexes)
- [Triggers](#triggers)
- [Stored Procedures](#stored-procedures)
## Installation
-   **Download MySQL** from the official website: [MySQL Downloads](https://dev.mysql.com/downloads/).
-   **Install MySQL** following the instructions for your operating system.
## Starting MySQL
-   **On Windows:** Use the MySQL Workbench or MySQL Command Line Client.
-   **On macOS/Linux:** Use the terminal.
```bash
mysql -u root -p
```
## Databases
**Create Database**  
Create a database named `my_database`.
```sql
CREATE DATABASE my_database;
```
**Use Database**  
    Select a specific database to work with and make it as the default database.
```sql
USE my_database;
```
**Drop Database**
Delete an existing database and all its contents.
```sql
DROP DATABASE my_database;
```
## Tables
**Create Table with Data Types**
Define a new table and a specific data types for each column.
```sql
CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100),
  age INT,
  email VARCHAR(100)
);
```
**Rename Table**
Change the name of an existing table.
```sql
RENAME TABLE users TO customers;
```
**Alter Table - Add Column**
Add a new column to an existing table.
```sql
ALTER TABLE users ADD COLUMN address VARCHAR(255);
```
**Modify Column Data Type**
Change the data type of an existing column.
```sql
ALTER TABLE users MODIFY COLUMN age SMALLINT;
```
**Add Column Before/After Another Column**
Add a new column in a specific position within the table.
```sql
ALTER TABLE users ADD COLUMN phone VARCHAR(15) AFTER email;
```
**Drop Column**
Remove an existing column from the table.
```sql
ALTER TABLE users DROP COLUMN address;
```
**Rename Column**
Change the name of a column in a table.
```sql
ALTER TABLE users CHANGE COLUMN phone contact_number VARCHAR(15);
```
## Insert Rows
Add a new row to the table.
```sql
INSERT INTO users (name, age, email) 
VALUES ('John Doe', 30, 'john@example.com');
```
Add multiple rows in a single query.
```sql
INSERT INTO users (name, age, email) 
VALUES 	('Alice', 25, 'alice@example.com'), 
		('Bob', 28, 'bob@example.com');
```
## Update & Delete
**Update Rows**
Modify data in existing rows based on a condition.
```sql
UPDATE users SET age = 35 WHERE name = 'John Doe';
```
**Update Multiple Columns**
Modifies multiple columns in a row.
```sql
UPDATE users SET age = 29, email = 'alice_new@example.com' WHERE name = 'Alice';
```
**Delete Rows**
Remove rows from a table based on a condition.
```sql
DELETE FROM users WHERE age < 20;
```
**Delete All Rows**
Delete all records from a table without deleting the table structure.
```sql
DELETE FROM users;
```
## Select
Retrieve specific columns from a table.
```sql
SELECT name, email FROM users;
```
Select rows based on a condition with `WHERE` Clause.
```sql
SELECT * FROM users WHERE age > 25;
```
Select rows where a column's value is `NULL`.
```sql
SELECT * FROM users WHERE email IS NULL;
```
Select rows where a column's value is `not NULL`.
```sql
SELECT * FROM users WHERE email IS NOT NULL;
```
Retrieve unique values from a column for eliminating any duplicates.
```sql
SELECT DISTINCT country FROM users;
```

## Autocommit, Commit, Rollback
**Disable Autocommit**
Turns off autocommit to allow transactions.
```sql
SET AUTOCOMMIT = 0;
```
**Commit**
Saves all changes made during the transaction.
```sql
COMMIT;
```
**Rollback** 
Reverts all changes made during the transaction.
```sql
ROLLBACK;
```
## Current Date & Time Functions
**Current Date**
Retrieves the current date.
```sql
SELECT CURRENT_DATE();
```
**Current Time**
Retrieves the current time.
```sql
SELECT CURRENT_TIME();
```
**Current DateTime**
Retrieves the current date and time.
```sql
SELECT NOW();
```
**Extract Date Parts**
Extracts specific parts from a date.
```sql
SELECT YEAR(NOW()), MONTH(NOW()), DAY(NOW());
```
## Constraints
**UNIQUE**
Ensures that all values in a column are unique.
```sql
CREATE TABLE users (
  email VARCHAR(100) UNIQUE
);
```
**NOT NULL**
Ensures that a column cannot have a `NULL` value.
```sql
CREATE TABLE users (
  name VARCHAR(100) NOT NULL
);
```
**CHECK**
Limits the values that can be placed in a column.
```sql
CREATE TABLE users (
  age INT CHECK(age >= 18)
);
```
**DEFAULT**
Provides a default value for a column if no value is specified.
```sql
CREATE TABLE users (
  country VARCHAR(50) DEFAULT 'USA'
);
```
**Primary Key**
Uniquely identifies each record in the table (not null and unique).
```sql
CREATE TABLE users (
  id INT PRIMARY KEY
);
```
**Auto Increment**
Automatically increments the primary key column (and you can select the increment value)
```sql
CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY
);
```
**Foreign Key**
Links two tables by establishing a relationship between them.
```sql
CREATE TABLE orders (
  id INT PRIMARY KEY,
  user_id INT,
  FOREIGN KEY (user_id) REFERENCES users(id)
);
```
## Joins
**Inner Join**
Combines rows from two tables where there is a match in both tables.

```sql
SELECT Customers.CustomerName, Orders.Product
FROM Customers
INNER JOIN Orders ON Customers.CustomerID = Orders.CustomerID;
```
**Left Join**
Returns all rows from the left table, even if there are no matches in the right table.
```sql
SELECT Customers.CustomerName, Orders.Product
FROM Customers
LEFT JOIN Orders ON Customers.CustomerID = Orders.CustomerID;
```
**Right Join**
Returns all rows from the right table, even if there are no matches in the left table.
```sql
SELECT Customers.CustomerName, Orders.Product
FROM Orders
RIGHT JOIN Customers ON Orders.CustomerID = Customers.CustomerID;
```
**Full Outer Join**
Returns rows when there is a match in either the left or right table.
```sql
SELECT Customers.CustomerName, Orders.Product
FROM Customers
FULL OUTER JOIN Orders ON Customers.CustomerID = Orders.CustomerID;
```
## Functions
**COUNT**
Counts the number of rows.
```sql
SELECT COUNT(*) FROM users;
```
**MAX & MIN**
Finds the maximum and minimum values in a column.
```sql
SELECT MAX(age), MIN(age) FROM users;
```
**AVG**
Calculates the average value of a column.
```sql
SELECT AVG(age) FROM users;
```
**SUM**
Calculates the total sum of a column's values.
```sql
SELECT SUM(age) FROM users;
```
**CONCAT** Combines two or more strings.
```sql
SELECT CONCAT(first_name, ' ', last_name) AS full_name FROM users;
```
**ROUND**
Rounds a number to a specified number of decimal places.
```sql
SELECT ROUND(AVG(age), 2) FROM users;
```
**UPPER & LOWER**
Converts a string to upper or lower case.
```sql
SELECT UPPER(name) FROM users;
```
## Logical Operators
**AND / OR**
Combines multiple conditions in a query.
```sql
SELECT * FROM users WHERE age > 25 AND country = 'USA';
```
**NOT**
Negates a condition.
```sql
SELECT * FROM users WHERE NOT (age < 18);
```
**BETWEEN**
Filters rows within a specified range.
```sql
SELECT * FROM users WHERE age BETWEEN 20 AND 30;
```
**IN**
Filters rows that match any value in a list.
```sql
SELECT * FROM users WHERE country IN ('USA', 'Canada');
```
## Wildcards
**LIKE %**
Searches for patterns in a column's value.
- Example: Fetches all users whose names start with the letter `J`.
```sql
SELECT * FROM users WHERE name LIKE 'J%';
```
**_ Wildcard (Single Character Match)**
Searches for patterns where a single character can vary.
- Example: Finds users whose names have 3 letters and start with `J` and end with `n`.
```sql
SELECT * FROM users WHERE name LIKE 'J_n';
```
## Order By
**Order by Ascending/Descending**
Sorts the result set in ascending (default) or descending order.
```sql
SELECT * FROM users ORDER BY age ASC;
SELECT * FROM users ORDER BY age DESC;
```
## Limit
Restricts the number of rows returned by the query.
- Example: Retrieves the first 10 users from the `users` table.
```sql
SELECT * FROM users LIMIT 10;
```
Skips a specific number of rows before starting to return the rows.
- Example: Retrieves 10 users, starting from the 6th user in the result set.
```sql
SELECT * FROM users LIMIT 10 OFFSET 5;
```
## DISTINCT
Used to remove duplicate records from the result set, returning only unique values.
- Example: Retrieve a list of unique countries from the `users` table, ensuring no duplicates.
```sql
SELECT DISTINCT country FROM users;
```
## On Delete (Cascade)
**Cascade Delete**
The `ON DELETE CASCADE` option ensures that when a record in the parent table is deleted, the related records in the child table are automatically deleted.
- Example: If a user is deleted from the `users` table, all related records in the `orders` table will also be deleted automatically.
```sql
CREATE TABLE orders (
  id INT PRIMARY KEY,
  user_id INT,
  FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);
```
## Group By & Rollup (HAVING)
**Group By**
Groups rows that have the same values in specified columns into aggregated data.
- Example: Counts the number of users in each country.
```sql
SELECT country, COUNT(*) AS user_count 
FROM users 
GROUP BY country;
```
**HAVING Clause**
Filters records after the `GROUP BY` operation (just like `where`).
- Example: Shows only countries with more than 10 users.
```sql
SELECT country, COUNT(*) AS user_count 
FROM users 
GROUP BY country 
HAVING user_count > 10;
```
**Rollup**
Provides subtotals and grand totals in grouped data.
- Example: Returns the number of users per country and adds a grand total of all users at the end.
```sql
SELECT country, COUNT(*) AS user_count 
FROM users 
GROUP BY country WITH ROLLUP;
```
## Unions
**Union Different Tables**
Combines the results of two or more queries into a single result set without any duplicates.
```sql
SELECT name FROM users
UNION
SELECT name FROM customers;
```
**Union All**
Similar to `UNION`, but includes duplicates.
```sql
SELECT name FROM users
UNION ALL
SELECT name FROM customers;
```
## Self Joins
**Join a Table with Itself**
Used to compare rows within the same table.
```sql
SELECT A.name, B.name
FROM employees A, employees B
WHERE A.manager_id = B.id;
```
## Subqueries
**Subquery in SELECT Statement**
A query nested within another query.
- Example1: Retrieves users whose age is above the average age of all users
```sql
SELECT name FROM users WHERE age > (SELECT AVG(age) FROM users);
```
- Example2: Fetches users who have placed at least one order.
```sql
SELECT name FROM users WHERE id IN (SELECT user_id FROM orders);
```
- Example3: Find customers who placed more than 5 orders.
```sql
SELECT customer_name
FROM Customers
WHERE customer_id IN (
    SELECT customer_id
    FROM Orders
    GROUP BY customer_id
    HAVING COUNT(order_id) > 5
);
```
## Views
**Create View**
Defines a virtual table based on a query.
- Example1: Create a view that shows all customers from 'New York'.
```sql
CREATE VIEW NY_Customers AS 
SELECT customer_id, customer_name, city FROM Customers WHERE city = 'New York';
```
- Example2: Creates a view called `user_details` that includes only the `name` and `age` columns from the `users` table.
```sql
CREATE VIEW user_details AS
SELECT name, age FROM users;
```
**Update a View**
Updates the data through a view.
```sql
UPDATE user_details SET age = 31 WHERE name = 'John Doe';
```
## Indexes
**Create Index**
Speeds up queries by creating an index on one or more columns.
```sql
CREATE INDEX idx_name ON users(name);
```
**Show Indexes**
Displays all indexes for a table.
```sql
SHOW INDEXES FROM users;
```
**Drop Index**
Removes an index from a table.
```sql
DROP INDEX idx_name ON users;
```

## Triggers
**Create a Trigger**
Executes a function automatically when a specific event occurs in a table.
- Example1: Automatically updates the `last_update` column to the current timestamp before any update on the `users` table.
 ```sql
CREATE TRIGGER update_time
BEFORE UPDATE ON users
FOR EACH ROW
SET NEW.last_update = NOW();
```
- Example2: Create a trigger that updates the stock when an order is placed
```sql
CREATE TRIGGER UpdateStockAfterOrder 
AFTER INSERT ON Orders 
FOR EACH ROW 
BEGIN 
UPDATE Products SET stock_quantity = stock_quantity - NEW.quantity 
WHERE product_id = NEW.product_id; 
END;
```
## Stored Procedures
**Create**
Is a set of SQL statements that can be executed as a single unit. You define it once and then call it whenever needed.
- Example1: This stored procedure, `getUsersByCountry`, retrieves all users from a specific country. The `IN` parameter `countryName` is used as input.
```sql
CREATE PROCEDURE getUsersByCountry(IN countryName VARCHAR(50))
BEGIN
  SELECT * FROM users WHERE country = countryName;
END;
```
**Call**
To execute a stored procedure, you use the `CALL` command.
- Example: Calls the `getUsersByCountry` procedure with the argument `'USA'`, fetching all users from the USA.
```sql
CALL getUsersByCountry('USA');
```
- Example2: Create a stored procedure to find the total orders of a customer
```sql
DELIMITER // 
CREATE PROCEDURE GetTotalOrders(IN cust_id INT) 
BEGIN 
SELECT COUNT(order_id) AS total_orders FROM Orders 
WHERE customer_id = cust_id; 
END // 
DELIMITER ;
```
```sql
CALL GetTotalOrders(1);
```
