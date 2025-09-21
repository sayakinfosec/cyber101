## SQL Fundamentals 

### 🔹 Introduction
- Databases store, organize, and manage data efficiently.  
- SQL (Structured Query Language) is the standard way to interact with relational databases.  
- Skills in SQL are essential for cybersecurity, data analysis, and development.

---

### 🔹 Databases 101
**Types of Databases:**
- **Relational (SQL):** Structured tables (rows/columns), strict schema.  
  Example: MySQL, PostgreSQL.  
- **Non-relational (NoSQL):** Flexible format (key-value, JSON, documents).  
  Example: MongoDB.

**Core Concepts:**
- **Table:** Collection of related data.  
- **Row (Record):** Individual entry in a table.  
- **Column (Field):** Attribute of data (with a data type).  

**Keys:**
- **Primary Key:** Uniquely identifies each row. (e.g., `id`)  
- **Foreign Key:** Creates relationships between tables.  

---

### 🔹 SQL
**SQL = Structured Query Language**  
- Used to **create, read, update, delete** (CRUD) data.  
- Managed through a **DBMS** (MySQL, Oracle, MariaDB).  

**Benefits of SQL:**
- Fast → handles large data quickly.  
- Easy → plain English-like syntax.  
- Reliable → strict structure enforces accuracy.  
- Flexible → powerful queries for analysis.  

---

### 🔹 Database and Table Statements
**Database Commands:**
```sql
CREATE DATABASE db_name;
SHOW DATABASES;
USE db_name;
DROP DATABASE db_name;
````

**Table Commands:**

```sql
CREATE TABLE table_name (
   id INT AUTO_INCREMENT PRIMARY KEY,
   name VARCHAR(255) NOT NULL,
   created_at DATE
);

SHOW TABLES;
DESCRIBE table_name;
ALTER TABLE table_name ADD column_name datatype;
DROP TABLE table_name;
```

---

### 🔹 CRUD Operations

**C → Create**

```sql
INSERT INTO users (id, name, email) VALUES (1, 'Alice', 'alice@mail.com');
```

**R → Read**

```sql
SELECT * FROM users;
SELECT name, email FROM users WHERE id = 1;
```

**U → Update**

```sql
UPDATE users SET email='new@mail.com' WHERE id=1;
```

**D → Delete**

```sql
DELETE FROM users WHERE id=1;
```

---

### 🔹 Clauses

**Filtering & Sorting Data**

* `WHERE` → filter rows
* `ORDER BY` → sort results
* `LIMIT` → restrict rows returned
* `GROUP BY` → aggregate rows
* `HAVING` → filter groups

**Examples:**

```sql
SELECT * FROM books WHERE publication_date > '2020-01-01';
SELECT * FROM books ORDER BY publication_date DESC;
SELECT COUNT(*) FROM books GROUP BY author_id HAVING COUNT(*) > 3;
```

---

### 🔹 Operators

**Comparison Operators:**

* `=`, `!=`, `<`, `>`, `<=`, `>=`

**Logical Operators:**

* `AND`, `OR`, `NOT`

**Pattern Matching:**

* `LIKE` → wildcard search (`%` for many chars, `_` for one char).

```sql
SELECT * FROM users WHERE name LIKE 'A%';
```

**IN / BETWEEN:**

```sql
SELECT * FROM users WHERE id IN (1, 2, 3);
SELECT * FROM books WHERE publication_date BETWEEN '2020-01-01' AND '2021-01-01';
```

---

Got it 👍 I’ll extend the notes and add a short **“Extra Examples”** section at the end of **Task 8 – Functions**. That way, the `GROUP_CONCAT` function and the `NOT LIKE "%0"` filter you solved are documented with context.

Here’s the updated section ⬇️

---

### 🔹 Functions

**Aggregate Functions:**
- `COUNT()`, `SUM()`, `AVG()`, `MIN()`, `MAX()`  
```sql
SELECT COUNT(*) FROM users;
SELECT AVG(price) FROM products;
````

**String Functions:**

* `CONCAT(a, b)`, `LENGTH(str)`, `UPPER(str)`, `LOWER(str)`

**Date/Time Functions:**

* `NOW()`, `CURDATE()`, `YEAR(date)`, `MONTH(date)`

**Special: GROUP\_CONCAT()**

* Combines multiple row values into one string, separated by commas (default) or a custom separator.

```sql
SELECT GROUP_CONCAT(name SEPARATOR ' & ')
FROM hacking_tools
WHERE amount NOT LIKE '%0';
```

💡 Example result:
`Flipper Zero & iCopy-XS`

---

### 🔹 Conclusion

* Databases power nearly all applications.
* SQL allows efficient **data storage, retrieval, and analysis**.
* Key mastery areas:

  * Creating databases & tables
  * CRUD operations
  * Clauses, operators, and functions

