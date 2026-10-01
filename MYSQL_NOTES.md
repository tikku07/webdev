# MySQL / SQL Notes

> Complete revision notes based on the uploaded MySQL tutorial.  
> Focus: practical SQL, MySQL syntax, database design, queries, joins, transactions, and advanced MySQL features.

---

## Table of Contents

1. [MySQL and DBMS Basics](#1-mysql-and-dbms-basics)
2. [Installation and MySQL Workbench](#2-installation-and-mysql-workbench)
3. [Databases and Tables](#3-databases-and-tables)
4. [Data Types](#4-data-types)
5. [Constraints](#5-constraints)
6. [SELECT and Filtering](#6-select-and-filtering)
7. [INSERT](#7-insert)
8. [UPDATE](#8-update)
9. [DELETE, DROP and TRUNCATE](#9-delete-drop-and-truncate)
10. [Aggregate and Built-in Functions](#10-aggregate-and-built-in-functions)
11. [Transactions](#11-transactions)
12. [Primary Keys and Auto Increment](#12-primary-keys-and-auto-increment)
13. [Foreign Keys](#13-foreign-keys)
14. [JOINs](#14-joins)
15. [UNION and UNION ALL](#15-union-and-union-all)
16. [Self JOIN](#16-self-join)
17. [Views](#17-views)
18. [Indexes](#18-indexes)
19. [Subqueries](#19-subqueries)
20. [GROUP BY and HAVING](#20-group-by-and-having)
21. [Stored Procedures](#21-stored-procedures)
22. [Triggers](#22-triggers)
23. [Other Important SQL Features](#23-other-important-sql-features)
24. [SQL Quick Revision](#24-sql-quick-revision)
25. [Interview Checklist](#25-interview-checklist)

---

# 1. MySQL and DBMS Basics

## What is MySQL?

**MySQL** is a relational database management system (RDBMS).

It is used to:
- Store data
- Retrieve data
- Insert data
- Update data
- Delete data
- Organize related data
- Maintain relationships between tables

## What is a DBMS?

A **Database Management System (DBMS)** is software that interacts with users, applications, and databases to store, retrieve, update, and manage data.

Examples of DBMS concepts are shared across systems such as MySQL and other relational databases.

---

# 2. Installation and MySQL Workbench

## MySQL Workbench

MySQL Workbench is a graphical tool used for:

- SQL development
- Database modeling
- Database administration
- Server configuration
- User administration
- Backup and management

## Windows / macOS

Typical installation:

1. Download MySQL Installer.
2. Choose **Developer Default**.
3. Set the root password.
4. Install MySQL Workbench if required.

## Ubuntu

Update packages:

```bash
sudo apt update
```

Install MySQL:

```bash
sudo apt install mysql-server
```

Secure installation:

```bash
sudo mysql_secure_installation
```

Log in:

```bash
sudo mysql
```

Create a user:

```sql
CREATE USER 'harry'@'localhost' IDENTIFIED BY 'password';

GRANT ALL PRIVILEGES ON *.* TO 'harry'@'localhost'
WITH GRANT OPTION;

FLUSH PRIVILEGES;

EXIT;
```

Test login:

```bash
mysql -u harry -p
```

> Use a strong password instead of the example password in real applications.

---

# 3. Databases and Tables

## What is a Database?

A database is an organized container for related data.

Simple analogy:

| Database concept | Excel analogy |
|---|---|
| Database | Workbook |
| Table | Sheet |
| Row | Row |
| Column | Column |

A database can contain multiple tables.

---

## Create a Database

```sql
CREATE DATABASE startersql;
```

Select it:

```sql
USE startersql;
```

Show databases:

```sql
SHOW DATABASES;
```

Delete a database:

```sql
DROP DATABASE startersql;
```

⚠️ `DROP DATABASE` deletes the database and its tables.

---

## Create a Table

```sql
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    gender ENUM('Male', 'Female', 'Other'),
    date_of_birth DATE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

View tables:

```sql
SHOW TABLES;
```

Describe a table:

```sql
DESCRIBE users;
```

---

# 4. Data Types

## Common MySQL Data Types

| Type | Purpose |
|---|---|
| `INT` | Whole numbers |
| `VARCHAR(n)` | Variable-length text |
| `DATE` | Date |
| `TIMESTAMP` | Date and time |
| `BOOLEAN` | TRUE/FALSE |
| `ENUM` | One value from a predefined list |
| `DECIMAL(p,s)` | Exact decimal values |

### Examples

```sql
age INT
```

```sql
name VARCHAR(100)
```

```sql
date_of_birth DATE
```

```sql
created_at TIMESTAMP
```

```sql
salary DECIMAL(10,2)
```

For `DECIMAL(10,2)`:

- `10` = total digits
- `2` = digits after decimal point

---

# 5. Constraints

Constraints are rules applied to columns to maintain valid and consistent data.

## Main Constraints

| Constraint | Purpose |
|---|---|
| `PRIMARY KEY` | Uniquely identifies a row |
| `FOREIGN KEY` | Links tables |
| `UNIQUE` | Prevents duplicate values |
| `NOT NULL` | Prevents NULL |
| `CHECK` | Restricts values using a condition |
| `DEFAULT` | Provides a default value |
| `AUTO_INCREMENT` | Automatically generates numbers |

---

## PRIMARY KEY

Uniquely identifies each row.

```sql
id INT PRIMARY KEY
```

A primary key:
- Must be unique
- Cannot be NULL
- Only one primary key constraint exists per table
- Can consist of one or multiple columns

---

## UNIQUE

Prevents duplicate values.

```sql
email VARCHAR(100) UNIQUE
```

Add later:

```sql
ALTER TABLE users
ADD CONSTRAINT unique_email UNIQUE (email);
```

---

## NOT NULL

Prevents a column from containing NULL.

```sql
name VARCHAR(100) NOT NULL
```

Add/change:

```sql
ALTER TABLE users
MODIFY COLUMN name VARCHAR(100) NOT NULL;
```

Allow NULL again:

```sql
ALTER TABLE users
MODIFY COLUMN name VARCHAR(100) NULL;
```

---

## CHECK

Restricts values using a condition.

```sql
ALTER TABLE users
ADD CONSTRAINT chk_dob
CHECK (date_of_birth > '2000-01-01');
```

---

## DEFAULT

Provides a value automatically when no value is supplied.

```sql
is_active BOOLEAN DEFAULT TRUE
```

---

## AUTO_INCREMENT

Automatically generates increasing numeric values.

```sql
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100)
);
```

---

# 6. SELECT and Filtering

## SELECT

Select all columns:

```sql
SELECT * FROM users;
```

Select specific columns:

```sql
SELECT name, email
FROM users;
```

Basic syntax:

```sql
SELECT column1, column2
FROM table_name;
```

---

## WHERE

Filters rows.

```sql
SELECT *
FROM users
WHERE gender = 'Male';
```

### Comparison Operators

| Operator | Meaning |
|---|---|
| `=` | Equal |
| `!=` | Not equal |
| `<>` | Not equal |
| `>` | Greater than |
| `<` | Less than |
| `>=` | Greater than or equal |
| `<=` | Less than or equal |

Examples:

```sql
SELECT * FROM users WHERE id > 10;
```

```sql
SELECT * FROM users WHERE id >= 5;
```

```sql
SELECT * FROM users WHERE id <= 20;
```

---

## NULL

Use:

```sql
SELECT *
FROM users
WHERE date_of_birth IS NULL;
```

Not NULL:

```sql
SELECT *
FROM users
WHERE date_of_birth IS NOT NULL;
```

Do not compare NULL using:

```sql
WHERE date_of_birth = NULL
```

---

## BETWEEN

```sql
SELECT *
FROM users
WHERE date_of_birth
BETWEEN '1990-01-01' AND '2000-12-31';
```

---

## IN

```sql
SELECT *
FROM users
WHERE gender IN ('Male', 'Other');
```

Equivalent idea:

```sql
WHERE gender = 'Male'
   OR gender = 'Other';
```

---

## LIKE

Used for pattern matching.

Starts with `A`:

```sql
SELECT *
FROM users
WHERE name LIKE 'A%';
```

Ends with `a`:

```sql
SELECT *
FROM users
WHERE name LIKE '%a';
```

Contains `li`:

```sql
SELECT *
FROM users
WHERE name LIKE '%li%';
```

### Wildcards

| Symbol | Meaning |
|---|---|
| `%` | Any sequence of characters |
| `_` | Exactly one character |

Example:

```sql
SELECT *
FROM users
WHERE name LIKE '_a%';
```

---

## AND / OR / NOT

### AND

All conditions must be true.

```sql
SELECT *
FROM users
WHERE gender = 'Female'
AND date_of_birth > '1990-01-01';
```

### OR

At least one condition must be true.

```sql
SELECT *
FROM users
WHERE gender = 'Male'
OR gender = 'Other';
```

### NOT

Reverses a condition.

```sql
SELECT *
FROM users
WHERE NOT gender = 'Female';
```

---

## ORDER BY

Ascending:

```sql
SELECT *
FROM users
ORDER BY date_of_birth ASC;
```

Descending:

```sql
SELECT *
FROM users
ORDER BY name DESC;
```

`ASC` = ascending  
`DESC` = descending

---

## LIMIT

```sql
SELECT *
FROM users
LIMIT 5;
```

Skip rows using OFFSET:

```sql
SELECT *
FROM users
LIMIT 10 OFFSET 5;
```

MySQL alternative:

```sql
SELECT *
FROM users
LIMIT 5, 10;
```

Format:

```text
LIMIT offset, count
```

Example:

```sql
SELECT *
FROM users
ORDER BY created_at DESC
LIMIT 10;
```

---

## DISTINCT

Returns unique values.

```sql
SELECT DISTINCT gender
FROM users;
```

---

# 7. INSERT

## Insert a Full Row

```sql
INSERT INTO users
VALUES (
    1,
    'Alice',
    'alice@example.com',
    'Female',
    '1995-05-14',
    DEFAULT
);
```

This depends on the exact table column order.

### Recommended approach

Specify columns explicitly:

```sql
INSERT INTO users
(name, email, gender, date_of_birth)
VALUES
('Bob', 'bob@example.com', 'Male', '1990-11-23');
```

This is safer when table structure changes.

---

## Insert Multiple Rows

```sql
INSERT INTO users
(name, email, gender, date_of_birth)
VALUES
('Bob', 'bob@example.com', 'Male', '1990-11-23'),
('Charlie', 'charlie@example.com', 'Other', '1988-02-17'),
('David', 'david@example.com', 'Male', '2000-08-09');
```

Multiple-row insertion is more efficient than inserting each row separately.

---

# 8. UPDATE

`UPDATE` modifies existing rows.

## Basic Syntax

```sql
UPDATE table_name
SET column1 = value1,
    column2 = value2
WHERE condition;
```

Example:

```sql
UPDATE users
SET name = 'Alicia'
WHERE id = 1;
```

Multiple columns:

```sql
UPDATE users
SET name = 'Robert',
    email = 'robert@example.com'
WHERE id = 2;
```

---

## Update Using an Expression

Increase salary:

```sql
UPDATE users
SET salary = salary + 10000
WHERE salary < 60000;
```

---

## ⚠️ UPDATE Without WHERE

```sql
UPDATE users
SET gender = 'Other';
```

This changes **every row**.

Always verify the target rows first:

```sql
SELECT *
FROM users
WHERE id = 1;
```

---

# 9. DELETE, DROP and TRUNCATE

## DELETE

Removes rows.

```sql
DELETE FROM users
WHERE id = 3;
```

Multiple rows:

```sql
DELETE FROM users
WHERE gender = 'Other';
```

Delete all rows but keep table:

```sql
DELETE FROM users;
```

---

## DELETE Without WHERE

```sql
DELETE FROM users;
```

This removes every row.

---

## DROP TABLE

```sql
DROP TABLE users;
```

Removes:
- Data
- Table structure

---

## TRUNCATE

```sql
TRUNCATE TABLE users;
```

Removes all rows while keeping the table structure.

The source notes that `TRUNCATE` is faster than deleting all rows and is generally not rollback-friendly.

---

## DELETE vs TRUNCATE vs DROP

| Command | Removes rows | Keeps structure |
|---|---:|---:|
| `DELETE` | Yes | Yes |
| `TRUNCATE` | All rows | Yes |
| `DROP` | Yes | No |

---

# 10. Aggregate and Built-in Functions

SQL functions help analyze, transform, or summarize data.

Assume:

```text
users(id, name, gender, salary, date_of_birth, created_at)
```

---

## Aggregate Functions

### COUNT

```sql
SELECT COUNT(*)
FROM users;
```

Count females:

```sql
SELECT COUNT(*)
FROM users
WHERE gender = 'Female';
```

---

## MIN and MAX

```sql
SELECT
    MIN(salary) AS min_salary,
    MAX(salary) AS max_salary
FROM users;
```

---

## SUM

```sql
SELECT SUM(salary) AS total_payroll
FROM users;
```

---

## AVG

```sql
SELECT AVG(salary) AS avg_salary
FROM users;
```

---

## GROUP BY

Average salary by gender:

```sql
SELECT
    gender,
    AVG(salary) AS avg_salary
FROM users
GROUP BY gender;
```

---

## String Functions

### LENGTH

```sql
SELECT
    name,
    LENGTH(name) AS name_length
FROM users;
```

### LOWER

```sql
SELECT
    name,
    LOWER(name) AS lowercase_name
FROM users;
```

### UPPER

```sql
SELECT
    name,
    UPPER(name) AS uppercase_name
FROM users;
```

### CONCAT

```sql
SELECT
    CONCAT(name, ' <', email, '>') AS user_contact
FROM users;
```

---

## Date Functions

### NOW

```sql
SELECT NOW();
```

### YEAR

```sql
SELECT
    name,
    YEAR(date_of_birth) AS birth_year
FROM users;
```

### MONTH / DAY

```sql
SELECT
    MONTH(date_of_birth),
    DAY(date_of_birth)
FROM users;
```

### DATEDIFF

```sql
SELECT
    name,
    DATEDIFF(CURDATE(), date_of_birth) AS days_lived
FROM users;
```

### TIMESTAMPDIFF

Calculate age:

```sql
SELECT
    name,
    TIMESTAMPDIFF(
        YEAR,
        date_of_birth,
        CURDATE()
    ) AS age
FROM users;
```

---

## Mathematical Functions

### ROUND

```sql
SELECT ROUND(salary)
FROM users;
```

### FLOOR

Rounds down.

```sql
SELECT FLOOR(salary)
FROM users;
```

### CEIL

Rounds up.

```sql
SELECT CEIL(salary)
FROM users;
```

### MOD

```sql
SELECT
    id,
    MOD(id, 2) AS remainder
FROM users;
```

Useful for checking even/odd values.

---

## Conditional Function: IF

```sql
SELECT
    name,
    gender,
    IF(gender = 'Female', 'Yes', 'No') AS is_female
FROM users;
```

---

## Function Quick Revision

| Function | Purpose |
|---|---|
| `COUNT()` | Count rows |
| `SUM()` | Total |
| `AVG()` | Average |
| `MIN()` | Minimum |
| `MAX()` | Maximum |
| `LENGTH()` | String length |
| `LOWER()` | Lowercase |
| `UPPER()` | Uppercase |
| `CONCAT()` | Combine strings |
| `NOW()` | Current date/time |
| `YEAR()` | Extract year |
| `DATEDIFF()` | Difference in days |
| `TIMESTAMPDIFF()` | Difference in specified unit |
| `ROUND()` | Round number |
| `IF()` | Conditional result |

---

# 11. Transactions

A **transaction** is a group of database operations treated as a unit.

MySQL normally operates with **AutoCommit** enabled.

## Disable AutoCommit

```sql
SET autocommit = 0;
```

Changes are not permanently saved until `COMMIT`.

---

## COMMIT

Saves changes.

```sql
COMMIT;
```

---

## ROLLBACK

Reverts changes since the previous commit/rollback point.

```sql
ROLLBACK;
```

---

## Example

```sql
SET autocommit = 0;

UPDATE users
SET salary = 80000
WHERE id = 5;
```

If correct:

```sql
COMMIT;
```

If incorrect:

```sql
ROLLBACK;
```

Enable AutoCommit again:

```sql
SET autocommit = 1;
```

---

# 12. Primary Keys and Auto Increment

## PRIMARY KEY

A primary key uniquely identifies a row.

Properties:

- Unique
- Cannot be NULL
- One primary-key constraint per table
- Can contain one or multiple columns

Example:

```sql
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100)
);
```

---

## PRIMARY KEY vs UNIQUE

| Feature | PRIMARY KEY | UNIQUE |
|---|---|---|
| Duplicate values | Not allowed | Not allowed |
| NULL | Not allowed | May allow NULL |
| Number per table | One primary-key constraint | Multiple unique constraints |
| Main row identifier | Yes | Usually no |

---

## Drop Primary Key

```sql
ALTER TABLE users
DROP PRIMARY KEY;
```

This may require handling dependencies such as foreign keys or auto-increment behavior.

---

## Auto Increment

```sql
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100)
);
```

Change starting value:

```sql
ALTER TABLE users
AUTO_INCREMENT = 1000;
```

---

# 13. Foreign Keys

A **foreign key** connects related tables.

Example:

```text
users
-----
id
name

addresses
---------
id
user_id
city
```

`addresses.user_id` can reference `users.id`.

---

## Create Foreign Key

```sql
CREATE TABLE addresses (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT,
    street VARCHAR(255),
    city VARCHAR(100),
    state VARCHAR(100),
    pincode VARCHAR(10),

    FOREIGN KEY (user_id)
        REFERENCES users(id)
);
```

The foreign key helps maintain valid references between tables.

---

## Named Foreign Key

```sql
CREATE TABLE addresses (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT,

    CONSTRAINT fk_user
        FOREIGN KEY (user_id)
        REFERENCES users(id)
);
```

---

## Add Foreign Key Later

```sql
ALTER TABLE addresses
ADD CONSTRAINT fk_user
FOREIGN KEY (user_id)
REFERENCES users(id);
```

---

## Drop Foreign Key

```sql
ALTER TABLE addresses
DROP FOREIGN KEY fk_user;
```

---

## ON DELETE

Controls what happens when a referenced parent row is deleted.

### CASCADE

```sql
FOREIGN KEY (user_id)
REFERENCES users(id)
ON DELETE CASCADE
```

Related child rows are automatically deleted.

### SET NULL

The foreign-key column becomes NULL.

### RESTRICT

Prevents deleting the parent when related child rows exist.

---

## ON DELETE Summary

| Option | Behavior |
|---|---|
| `CASCADE` | Delete related child rows |
| `SET NULL` | Set child foreign key to NULL |
| `RESTRICT` | Prevent parent deletion |

---

# 14. JOINs

JOINs combine rows from multiple tables based on related columns.

Assume:

### users

| id | name |
|---:|---|
| 1 | Aarav |
| 2 | Sneha |
| 3 | Raj |

### addresses

| id | user_id | city |
|---:|---:|---|
| 1 | 1 | Mumbai |
| 2 | 2 | Kolkata |
| 3 | 4 | Delhi |

---

## INNER JOIN

Returns only matching rows.

```sql
SELECT
    users.name,
    addresses.city
FROM users
INNER JOIN addresses
    ON users.id = addresses.user_id;
```

Result:

```text
Aarav  Mumbai
Sneha  Kolkata
```

Raj is excluded because no matching address exists.

---

## LEFT JOIN

Returns every row from the left table and matching rows from the right table.

```sql
SELECT
    users.name,
    addresses.city
FROM users
LEFT JOIN addresses
    ON users.id = addresses.user_id;
```

Result:

```text
Aarav   Mumbai
Sneha   Kolkata
Raj     NULL
```

---

## RIGHT JOIN

Returns every row from the right table and matching rows from the left table.

```sql
SELECT
    users.name,
    addresses.city
FROM users
RIGHT JOIN addresses
    ON users.id = addresses.user_id;
```

Result:

```text
Aarav   Mumbai
Sneha   Kolkata
NULL    Delhi
```

---

## JOIN Summary

| JOIN | Returns |
|---|---|
| `INNER JOIN` | Matching rows only |
| `LEFT JOIN` | All left + matching right |
| `RIGHT JOIN` | All right + matching left |

### Easy memory trick

```text
INNER → only matches
LEFT  → keep everything on LEFT
RIGHT → keep everything on RIGHT
```

---

# 15. UNION and UNION ALL

`UNION` combines the results of multiple `SELECT` statements.

## UNION

Removes duplicates.

```sql
SELECT name
FROM users

UNION

SELECT name
FROM admin_users;
```

---

## UNION ALL

Keeps duplicates.

```sql
SELECT name
FROM users

UNION ALL

SELECT name
FROM admin_users;
```

---

## Multiple Columns

Both SELECT statements should return the same number of columns with compatible types.

```sql
SELECT name, salary
FROM users

UNION

SELECT name, salary
FROM admin_users;
```

---

## Add a Role

```sql
SELECT name, 'User' AS role
FROM users

UNION

SELECT name, 'Admin' AS role
FROM admin_users;
```

---

## ORDER BY with UNION

```sql
SELECT name
FROM users

UNION

SELECT name
FROM admin_users

ORDER BY name;
```

---

## UNION Rules

1. Same number of columns.
2. Corresponding data types should be compatible.
3. `UNION` removes duplicates.
4. `UNION ALL` keeps duplicates.

---

# 16. Self JOIN

A **Self JOIN** joins a table with itself.

Useful when rows in the same table are related.

Example: users referring other users.

Add a column:

```sql
ALTER TABLE users
ADD COLUMN referred_by_id INT;
```

Example data:

```text
User 1 → referred nobody
User 2 → referred by User 1
User 3 → referred by User 1
User 4 → referred by User 2
```

Update:

```sql
UPDATE users
SET referred_by_id = 1
WHERE id IN (2, 3);

UPDATE users
SET referred_by_id = 2
WHERE id = 4;
```

Query:

```sql
SELECT
    a.id,
    a.name AS user_name,
    b.name AS referred_by
FROM users a
LEFT JOIN users b
    ON a.referred_by_id = b.id;
```

Here:

- `a` = current user
- `b` = referring user

Aliases are important because the same table is used twice.

---

# 17. Views

A **view** is a virtual table created from a `SELECT` query.

It does not duplicate the underlying data.

Useful for:

- Reusing queries
- Simplifying complex queries
- Filtering data
- Hiding selected columns
- Creating reusable database views

---

## Create View

```sql
CREATE VIEW high_salary_users AS
SELECT
    id,
    name,
    salary
FROM users
WHERE salary > 70000;
```

Query:

```sql
SELECT *
FROM high_salary_users;
```

The view reflects current data from the underlying table.

---

## Drop View

```sql
DROP VIEW high_salary_users;
```

### Key Point

Think of a view as a **saved SELECT query**.

---

# 18. Indexes

An **index** improves data retrieval performance.

Think of it like the index of a book.

Indexes are useful for columns frequently used in:

- `WHERE`
- `JOIN`
- `ORDER BY`

---

## Show Indexes

```sql
SHOW INDEXES FROM users;
```

---

## Create an Index

```sql
CREATE INDEX idx_email
ON users(email);
```

This can improve queries such as:

```sql
SELECT *
FROM users
WHERE email = 'example@example.com';
```

---

## Important Trade-off

Indexes:

- Improve reads
- Consume disk space
- Add overhead to `INSERT`
- Add overhead to `UPDATE`
- Add overhead to `DELETE`

Do not create indexes unnecessarily.

---

## Composite / Multi-Column Index

```sql
CREATE INDEX idx_gender_salary
ON users(gender, salary);
```

Useful for queries such as:

```sql
SELECT *
FROM users
WHERE gender = 'Female'
AND salary > 70000;
```

---

## Index Order Matters

For:

```sql
INDEX(gender, salary)
```

A query filtering on `gender` can use the leading part of the index effectively.

A query filtering only on `salary` may not use the composite index as effectively.

This is related to the **leftmost/leading-column principle** of composite indexes.

---

## Drop Index

```sql
DROP INDEX idx_email
ON users;
```

---

# 19. Subqueries

A **subquery** is a query inside another query.

Common locations:

- `WHERE`
- `SELECT`
- `FROM`

---

## Scalar Subquery

Find users earning more than the average salary:

```sql
SELECT
    id,
    name,
    salary
FROM users
WHERE salary > (
    SELECT AVG(salary)
    FROM users
);
```

The inner query returns one value.

---

## Subquery with IN

Find users referred by people earning more than ₹75,000:

```sql
SELECT
    id,
    name,
    referred_by_id
FROM users
WHERE referred_by_id IN (
    SELECT id
    FROM users
    WHERE salary > 75000
);
```

The inner query returns multiple IDs.

---

## Subquery in SELECT

```sql
SELECT
    name,
    salary,
    (
        SELECT AVG(salary)
        FROM users
    ) AS average_salary
FROM users;
```

Each row can be shown alongside the overall average.

---

## Subquery Types

| Type | Purpose |
|---|---|
| Scalar subquery | Returns one value |
| `IN` subquery | Returns multiple values |
| Subquery in SELECT | Produces calculated value |
| Subquery in FROM | Acts as a derived/virtual table |

---

# 20. GROUP BY and HAVING

## GROUP BY

Groups rows with the same value.

Usually used with:

- `COUNT`
- `SUM`
- `AVG`
- `MIN`
- `MAX`

Example:

```sql
SELECT
    gender,
    AVG(salary) AS average_salary
FROM users
GROUP BY gender;
```

---

## GROUP BY with COUNT

Count referrals:

```sql
SELECT
    referred_by_id,
    COUNT(*) AS total_referred
FROM users
WHERE referred_by_id IS NOT NULL
GROUP BY referred_by_id;
```

---

## HAVING

`HAVING` filters groups after aggregation.

Example:

```sql
SELECT
    gender,
    AVG(salary) AS avg_salary
FROM users
GROUP BY gender
HAVING AVG(salary) > 75000;
```

---

## WHERE vs HAVING

| Clause | Works on | Used |
|---|---|---|
| `WHERE` | Individual rows | Before grouping |
| `GROUP BY` | Groups rows | Creates groups |
| `HAVING` | Groups | After aggregation |

### Important

Use:

```sql
WHERE salary > 50000
```

when filtering individual rows.

Use:

```sql
HAVING AVG(salary) > 75000
```

when filtering an aggregate result.

---

## HAVING with COUNT

```sql
SELECT
    referred_by_id,
    COUNT(*) AS total_referred
FROM users
WHERE referred_by_id IS NOT NULL
GROUP BY referred_by_id
HAVING COUNT(*) > 1;
```

---

## ROLLUP

Used to generate subtotals and a grand total.

```sql
SELECT
    gender,
    COUNT(*) AS total_users
FROM users
GROUP BY gender WITH ROLLUP;
```

---

# 21. Stored Procedures

A **stored procedure** is a saved SQL program that can be executed later.

Useful for reusable database logic.

---

## Why DELIMITER?

Normally SQL statements end with:

```text
;
```

A procedure can contain many statements that also use `;`.

Therefore the delimiter is temporarily changed.

---

## Create Procedure

```sql
DELIMITER $$

CREATE PROCEDURE procedure_name()
BEGIN
    -- SQL statements
END$$

DELIMITER ;
```

---

## Procedure with Parameters

```sql
DELIMITER $$

CREATE PROCEDURE AddUser(
    IN p_name VARCHAR(100),
    IN p_email VARCHAR(100),
    IN p_gender ENUM('Male', 'Female', 'Other'),
    IN p_dob DATE,
    IN p_salary INT
)
BEGIN
    INSERT INTO users
        (name, email, gender, date_of_birth, salary)
    VALUES
        (p_name, p_email, p_gender, p_dob, p_salary);
END$$

DELIMITER ;
```

---

## Call Procedure

```sql
CALL AddUser(
    'Kiran Sharma',
    'kiran@example.com',
    'Female',
    '1994-06-15',
    72000
);
```

---

## IN Parameter

`IN` specifies an input parameter.

Example:

```sql
IN p_name VARCHAR(100)
```

---

## Show Procedures

```sql
SHOW PROCEDURE STATUS
WHERE Db = 'startersql';
```

---

## Drop Procedure

```sql
DROP PROCEDURE IF EXISTS AddUser;
```

---

# 22. Triggers

A **trigger** is a database program that automatically executes when a specified event occurs.

Events:

- `INSERT`
- `UPDATE`
- `DELETE`

Timing:

- `BEFORE`
- `AFTER`

Uses:

- Logging changes
- Enforcing business rules
- Automatically updating related data

---

## Basic Structure

```sql
CREATE TRIGGER trigger_name
AFTER INSERT ON table_name
FOR EACH ROW
BEGIN
    -- statements
END;
```

---

## NEW and OLD

| Keyword | Meaning |
|---|---|
| `NEW.column` | New row value |
| `OLD.column` | Previous row value |

Typical availability:

- `INSERT` → `NEW`
- `UPDATE` → `NEW` and `OLD`
- `DELETE` → `OLD`

---

## Example: User Log

Create log table:

```sql
CREATE TABLE user_log (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT,
    name VARCHAR(100),
    created_on TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

Create trigger:

```sql
DELIMITER $$

CREATE TRIGGER after_user_insert
AFTER INSERT ON users
FOR EACH ROW
BEGIN
    INSERT INTO user_log (user_id, name)
    VALUES (NEW.id, NEW.name);
END$$

DELIMITER ;
```

When a user is inserted, a log entry is automatically created.

---

## Test

```sql
CALL AddUser(
    'Ritika Jain',
    'ritika@example.com',
    'Female',
    '1996-03-12',
    74000
);
```

Check:

```sql
SELECT *
FROM user_log;
```

---

## Drop Trigger

```sql
DROP TRIGGER IF EXISTS after_user_insert;
```

---

# 23. Other Important SQL Features

## Logical Operators

| Operator | Meaning |
|---|---|
| `AND` | All conditions must be true |
| `OR` | At least one condition is true |
| `NOT` | Reverses condition |

Example:

```sql
SELECT *
FROM users
WHERE salary > 50000
AND gender = 'Male';
```

---

## ALTER TABLE

Add a column:

```sql
ALTER TABLE users
ADD COLUMN city VARCHAR(100);
```

---

## CHANGE vs MODIFY

### CHANGE

Can rename a column and change its data type.

```sql
ALTER TABLE users
CHANGE COLUMN city location VARCHAR(150);
```

### MODIFY

Changes the definition/type without renaming.

```sql
ALTER TABLE users
MODIFY COLUMN salary BIGINT;
```

---

## Rename Table

```sql
RENAME TABLE users TO customers;
```

Rename back:

```sql
RENAME TABLE customers TO users;
```

---

## Add Column

```sql
ALTER TABLE users
ADD COLUMN is_active BOOLEAN DEFAULT TRUE;
```

---

## Drop Column

```sql
ALTER TABLE users
DROP COLUMN is_active;
```

---

## Move Column Position

Move to first:

```sql
ALTER TABLE users
MODIFY COLUMN email VARCHAR(100) FIRST;
```

Move after another column:

```sql
ALTER TABLE users
MODIFY COLUMN gender
ENUM('Male', 'Female', 'Other')
AFTER name;
```

---

# 24. SQL Quick Revision

## Database Commands

```sql
CREATE DATABASE db_name;

USE db_name;

SHOW DATABASES;

DROP DATABASE db_name;
```

---

## Table Commands

```sql
CREATE TABLE table_name (...);

SHOW TABLES;

DESCRIBE table_name;

DROP TABLE table_name;
```

---

## CRUD

### CREATE / INSERT

```sql
INSERT INTO users (name, email)
VALUES ('Alice', 'alice@example.com');
```

### READ

```sql
SELECT *
FROM users;
```

### UPDATE

```sql
UPDATE users
SET name = 'Alicia'
WHERE id = 1;
```

### DELETE

```sql
DELETE FROM users
WHERE id = 1;
```

---

## Filtering

```sql
WHERE
AND
OR
NOT
IN
BETWEEN
LIKE
IS NULL
IS NOT NULL
```

---

## Sorting and Limiting

```sql
ORDER BY column ASC;

ORDER BY column DESC;

LIMIT 10;

LIMIT 10 OFFSET 20;
```

---

## Aggregation

```sql
COUNT()
SUM()
AVG()
MIN()
MAX()
```

---

## Grouping

```sql
GROUP BY
HAVING
WITH ROLLUP
```

---

## Relationships

```text
PRIMARY KEY
FOREIGN KEY
INNER JOIN
LEFT JOIN
RIGHT JOIN
SELF JOIN
```

---

## Advanced Features

```text
UNION
UNION ALL
SUBQUERIES
VIEWS
INDEXES
TRANSACTIONS
STORED PROCEDURES
TRIGGERS
```

---

# 25. Interview Checklist

## Basic Questions

- What is MySQL?
- What is DBMS?
- What is RDBMS?
- What is a database?
- What is a table?
- What is a row?
- What is a column?
- What is a primary key?
- What is a foreign key?
- What is a constraint?

## Query Questions

- What is `SELECT`?
- How does `WHERE` work?
- Difference between `WHERE` and `HAVING`.
- Difference between `DELETE`, `TRUNCATE`, and `DROP`.
- Difference between `UNION` and `UNION ALL`.
- What is `GROUP BY`?
- What is `ORDER BY`?
- What is `DISTINCT`?
- What is a subquery?

## JOIN Questions

- What is a JOIN?
- What is an INNER JOIN?
- What is a LEFT JOIN?
- What is a RIGHT JOIN?
- What is a SELF JOIN?
- When would you use a foreign key?

## Database Design

- Why use primary keys?
- Why use foreign keys?
- Why normalize data into multiple tables?
- What is an index?
- What are the benefits and costs of indexes?

## Advanced MySQL

- What is a transaction?
- What is `COMMIT`?
- What is `ROLLBACK`?
- What is AutoCommit?
- What is a view?
- What is a stored procedure?
- Why is `DELIMITER` used?
- What is a trigger?
- What are `NEW` and `OLD` in triggers?

---

# Must-Remember SQL Patterns

## Find rows matching a condition

```sql
SELECT *
FROM users
WHERE salary > 50000;
```

## Sort results

```sql
SELECT *
FROM users
ORDER BY salary DESC;
```

## Top N rows

```sql
SELECT *
FROM users
ORDER BY salary DESC
LIMIT 5;
```

## Count rows

```sql
SELECT COUNT(*)
FROM users;
```

## Average by category

```sql
SELECT
    gender,
    AVG(salary)
FROM users
GROUP BY gender;
```

## Filter aggregate results

```sql
SELECT
    gender,
    AVG(salary)
FROM users
GROUP BY gender
HAVING AVG(salary) > 75000;
```

## Join two tables

```sql
SELECT
    u.name,
    a.city
FROM users u
INNER JOIN addresses a
    ON u.id = a.user_id;
```

## Find values above average

```sql
SELECT *
FROM users
WHERE salary > (
    SELECT AVG(salary)
    FROM users
);
```

## Create an index

```sql
CREATE INDEX idx_email
ON users(email);
```

## Transaction

```sql
SET autocommit = 0;

UPDATE users
SET salary = salary + 5000
WHERE id = 1;

COMMIT;
```

---

# SQL Learning Order

For practical developer preparation, learn in this order:

```text
1. Database + Table
        ↓
2. Data Types + Constraints
        ↓
3. SELECT
        ↓
4. WHERE / AND / OR / IN / BETWEEN / LIKE
        ↓
5. ORDER BY / LIMIT / DISTINCT
        ↓
6. INSERT
        ↓
7. UPDATE
        ↓
8. DELETE
        ↓
9. Aggregate Functions
        ↓
10. GROUP BY + HAVING
        ↓
11. Primary Key + Foreign Key
        ↓
12. JOINs
        ↓
13. Subqueries
        ↓
14. UNION
        ↓
15. Views
        ↓
16. Indexes
        ↓
17. Transactions
        ↓
18. Stored Procedures
        ↓
19. Triggers
```

---

# Final Revision Sheet

### Core commands

```text
CREATE
USE
INSERT
SELECT
UPDATE
DELETE
ALTER
DROP
TRUNCATE
```

### Filtering

```text
WHERE
AND
OR
NOT
IN
BETWEEN
LIKE
IS NULL
```

### Result control

```text
ORDER BY
LIMIT
OFFSET
DISTINCT
```

### Aggregation

```text
COUNT
SUM
AVG
MIN
MAX
GROUP BY
HAVING
ROLLUP
```

### Relationships

```text
PRIMARY KEY
FOREIGN KEY
INNER JOIN
LEFT JOIN
RIGHT JOIN
SELF JOIN
```

### Advanced

```text
UNION
UNION ALL
SUBQUERY
VIEW
INDEX
TRANSACTION
COMMIT
ROLLBACK
STORED PROCEDURE
TRIGGER
```

> **Goal:** Be able to read a database schema, write CRUD queries, filter and aggregate data, join related tables, use subqueries, and understand basic database performance and transaction concepts.
