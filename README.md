# SQL Commands Reference Guide

This document provides a comprehensive overview of SQL commands categorized into Data Manipulation Language (DML), Transaction Control Language (TCL), Data Control Language (DCL), Data Definition Language (DDL), and Data Query Language (DQL), complete with multiple examples for each.

---

## 1. DML (Data Manipulation Language)

### INSERT
Adds new rows of data into a table.
```sql
-- Example 1: Insert into specific columns
INSERT INTO employees (id, name, department) VALUES (1, 'Alice', 'HR');

-- Example 2: Insert into all columns without specifying names
INSERT INTO employees VALUES (2, 'Bob', 'IT', 60000);

-- Example 3: Insert multiple rows at once
INSERT INTO employees (id, name) VALUES (3, 'Charlie'), (4, 'David');
```

### UPDATE
Modifies existing data in a table.
```sql
-- Example 1: Update a single column for a specific row
UPDATE employees SET salary = 65000 WHERE name = 'Alice';

-- Example 2: Update multiple columns at once
UPDATE employees SET department = 'Sales', salary = 70000 WHERE id = 2;

-- Example 3: Update all rows (Use with caution!)
UPDATE employees SET status = 'Active';
```

### DELETE
Removes rows from a table.
```sql
-- Example 1: Delete a specific row
DELETE FROM employees WHERE id = 4;

-- Example 2: Delete based on a condition
DELETE FROM employees WHERE salary < 50000;

-- Example 3: Delete all rows from a table (table structure remains)
DELETE FROM employees;
```

---

## 2. TCL (Transaction Control Language)

### COMMIT
Saves all transaction changes permanently to the database.
```sql
-- Example: Save an update permanently
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;
COMMIT;
```

### ROLLBACK
Undoes transactions that have not yet been saved to the database.
```sql
-- Example: Revert accidental changes
BEGIN;
DELETE FROM employees WHERE department = 'HR';
ROLLBACK; -- The deleted rows are instantly restored
```

### SAVEPOINT
Sets a point within a transaction to which you can later roll back.
```sql
-- Example: Using savepoints to roll back partial transactions
BEGIN;
INSERT INTO orders (id, item) VALUES (1, 'Laptop');
SAVEPOINT sp1;
INSERT INTO orders (id, item) VALUES (2, 'Mouse');
ROLLBACK TO sp1; -- Undoes the 'Mouse' insert, but keeps the 'Laptop'
COMMIT;
```

---

## 3. DCL (Data Control Language)

### GRANT
Gives user access privileges to a database.
```sql
-- Example 1: Grant specific permissions to a user
GRANT SELECT, INSERT ON employees TO 'user1'@'localhost';

-- Example 2: Grant all permissions to an admin
GRANT ALL PRIVILEGES ON database_name.* TO 'admin'@'localhost';
```

### REVOKE
Withdraws user access privileges.
```sql
-- Example 1: Revoke specific permissions
REVOKE INSERT ON employees FROM 'user1'@'localhost';

-- Example 2: Revoke all permissions
REVOKE ALL PRIVILEGES ON database_name.* FROM 'admin'@'localhost';
```

---

## 4. DDL (Data Definition Language)

### CREATE TABLE
Creates a new table in the database.
```sql
-- Example: Create a new students table
CREATE TABLE students (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    age INT,
    enrollment_date DATE
);
```

### DROP TABLE
Deletes an entire table and all its data.
```sql
-- Example 1: Drop a table permanently
DROP TABLE students;

-- Example 2: Drop only if it exists to avoid errors
DROP TABLE IF EXISTS old_backup_table;
```

### RENAME TABLE
Changes the name of an existing table.
```sql
-- Example 1: Rename a table (MySQL syntax)
RENAME TABLE students TO alumni;

-- Example 2: Rename a table (PostgreSQL/SQL Server syntax)
ALTER TABLE students RENAME TO alumni;
```

### TRUNCATE TABLE
Quickly removes all rows from a table, but keeps the structure and resets auto-increment values.
```sql
-- Example: Empty a table completely
TRUNCATE TABLE employees;
```

### ALTER TABLE Commands

**ADD COLUMN**
```sql
ALTER TABLE employees ADD email VARCHAR(100);
```

**DROP COLUMN**
```sql
ALTER TABLE employees DROP COLUMN email;
```

**RENAME COLUMN**
```sql
-- Example (MySQL 8.0+ / PostgreSQL):
ALTER TABLE employees RENAME COLUMN name TO full_name;
```

**MODIFY COLUMN / CHANGE DATA-TYPE/SIZE**
```sql
-- Example (MySQL): Change datatype size
ALTER TABLE employees MODIFY COLUMN salary DECIMAL(10, 2);

-- Example (PostgreSQL): Change datatype
ALTER TABLE employees ALTER COLUMN salary TYPE DECIMAL(10,2);
```

**ADD/DROP PRIMARY KEY**
```sql
-- Add Primary Key
ALTER TABLE employees ADD PRIMARY KEY (id);

-- Drop Primary Key (MySQL)
ALTER TABLE employees DROP PRIMARY KEY;
```

**ADD/DROP FOREIGN KEY**
```sql
-- Add Foreign Key
ALTER TABLE orders ADD CONSTRAINT fk_customer FOREIGN KEY (customer_id) REFERENCES customers(id);

-- Drop Foreign Key (MySQL)
ALTER TABLE orders DROP FOREIGN KEY fk_customer;
```

**ADD/DROP UNIQUE KEY**
```sql
-- Add Unique Key
ALTER TABLE employees ADD CONSTRAINT uni_email UNIQUE (email);

-- Drop Unique Key (MySQL)
ALTER TABLE employees DROP INDEX uni_email;
```

**ADD/DROP CHECK**
```sql
-- Add Check Constraint
ALTER TABLE employees ADD CONSTRAINT chk_age CHECK (age >= 18);

-- Drop Check Constraint (MySQL)
ALTER TABLE employees DROP CHECK chk_age;
```

**MAKE NULLABLE OR NOT NULL**
```sql
-- Set column to NOT NULL (MySQL)
ALTER TABLE employees MODIFY name VARCHAR(50) NOT NULL;

-- Allow NULL values (MySQL)
ALTER TABLE employees MODIFY name VARCHAR(50) NULL;
```

---

## 5. DQL (Data Query Language)

### Clauses

**SELECT & FROM**
Used to retrieve data from a database.
```sql
-- Example 1: Select all columns
SELECT * FROM employees;

-- Example 2: Select specific columns
SELECT name, department FROM employees;
```

**WHERE**
Filters records based on specific conditions.
```sql
-- Example: Filter by string condition
SELECT * FROM employees WHERE department = 'IT';
```

**GROUP BY & HAVING**
Groups rows sharing a property and applies a condition to the aggregated group.
```sql
-- Example: Count employees per department having more than 5 employees
SELECT department, COUNT(*) 
FROM employees 
GROUP BY department 
HAVING COUNT(*) > 5;
```

**ORDER BY**
Sorts the result-set in ascending or descending order.
```sql
-- Example 1: Sort by salary descending
SELECT * FROM employees ORDER BY salary DESC;

-- Example 2: Sort by name ascending
SELECT * FROM employees ORDER BY name ASC;
```

**JOIN**
Combines rows from two or more tables based on a related column.
```sql
-- Example: Inner Join
SELECT e.name, d.department_name
FROM employees e
JOIN departments d ON e.dept_id = d.id;
```

### Operators

**BETWEEN**
Selects values within a given range.
```sql
SELECT * FROM employees WHERE salary BETWEEN 50000 AND 80000;
```

**LIKE**
Searches for a specified pattern in a column using wildcards (`%` and `_`).
```sql
-- Example: Names starting with 'A'
SELECT * FROM employees WHERE name LIKE 'A%';

-- Example: Names containing 'son'
SELECT * FROM employees WHERE name LIKE '%son%';
```

**IN**
Allows specifying multiple values in a WHERE clause (acts as multiple OR conditions).
```sql
SELECT * FROM employees WHERE department IN ('IT', 'HR', 'Sales');
```

**IS NULL**
Tests for empty (NULL) values.
```sql
-- Example 1: Find rows where manager_id is NULL
SELECT * FROM employees WHERE manager_id IS NULL;

-- Example 2: Find rows where manager_id is NOT NULL
SELECT * FROM employees WHERE manager_id IS NOT NULL;
```

**UNION**
Combines the result-set of two or more SELECT statements (removes duplicates).
```sql
SELECT city FROM customers
UNION
SELECT city FROM suppliers;
```

### Functions

**Aggregate Functions (COUNT, SUM, MIN, MAX, AVG)**
```sql
SELECT COUNT(*) FROM employees;             -- Counts total rows
SELECT SUM(salary) FROM employees;          -- Calculates total sum
SELECT MIN(age), MAX(age) FROM employees;   -- Finds smallest & largest values
SELECT AVG(salary) FROM employees;          -- Calculates average value
```

**String Functions (UPPER, LOWER, LENGTH, SUBSTRING, CONCAT)**
```sql
SELECT UPPER(name) FROM employees;          -- Converts to UPPERCASE
SELECT LOWER(name) FROM employees;          -- Converts to lowercase
SELECT LENGTH(name) FROM employees;         -- Gets number of characters
SELECT SUBSTRING(name, 1, 3) FROM employees;-- Extracts first 3 characters
SELECT CONCAT(first_name, ' ', last_name) FROM employees; -- Combines strings
```

**Math Functions (ROUND, CEIL, FLOOR, POWER, SQRT)**
```sql
SELECT ROUND(salary, 2) FROM employees;     -- Rounds to 2 decimal places
SELECT CEIL(4.2);                           -- Rounds up to nearest integer (5)
SELECT FLOOR(4.8);                          -- Rounds down to nearest integer (4)
SELECT POWER(2, 3);                         -- Calculates 2 raised to 3 (8)
SELECT SQRT(16);                            -- Calculates square root (4)
```

**Date Functions (NOW, DATE_FORMAT, DATE_ADD, DATE_DIFF)**
```sql
SELECT NOW();                               -- Gets current system date and time
SELECT DATE_FORMAT(NOW(), '%Y-%m-%d');      -- Formats date as YYYY-MM-DD
SELECT DATE_ADD(NOW(), INTERVAL 7 DAY);     -- Adds 7 days to current date
SELECT DATEDIFF('2026-12-31', NOW());       -- Calculates difference in days
```
