
### A. Logging In and Connecting

To start, you usually log in as the default administrator to set up users and databases.

Bash

```bash
# 1. Log in as the default Postgres admin user
sudo -u postgres psql

# 2. Connect directly to a specific user and database from the shell
psql -U myuser -d mydb -h localhost

# 3. Connect and be prompted for the password
psql -U myuser -W mydb
```

### B. Exit

Bash

```bash
# Exit the psql session and return to the Linux shell
\q
```

---

## 2. 👤 User & Database Management (Inside `psql`)

These commands are executed **inside the `psql` console**.

### A. Creating the Environment

SQL

```bash
-- 1. Create a new user (role)
CREATE USER myuser WITH PASSWORD 'strongpassword';

-- 2. Create a new database, specifying the owner
CREATE DATABASE mydb OWNER myuser;

-- 3. Grant all privileges on the new database to the user
GRANT ALL PRIVILEGES ON DATABASE mydb TO myuser;
```

### B. Connection & Inspection Meta-Commands

SQL

```bash
-- Connect to the new database
\c mydb

-- List all databases
\l

-- List all users (roles)
\du
```

---

## 3. 🏗️ Data Definition Language (DDL) - Table Creation

These commands create and modify the structural schema of the database.

### A. Creating a Table

SQL

```bash
CREATE TABLE products (
    id SERIAL PRIMARY KEY,              -- Auto-incrementing unique identifier
    name VARCHAR(100) NOT NULL,         -- Product name, cannot be NULL
    price NUMERIC(10, 2) DEFAULT 0.00,  -- Price, up to 10 digits total, 2 after decimal
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### B. Modifying and Deleting Tables

SQL

```bash
-- Add a new column to an existing table
ALTER TABLE products ADD COLUMN description TEXT;

-- Rename an existing column
ALTER TABLE products RENAME COLUMN name TO product_name;

-- Drop (delete) a column
ALTER TABLE products DROP COLUMN price;

-- Permanently delete the table and all its data
DROP TABLE products;
```

### C. Inspection Meta-Commands

SQL

```bash
-- List all tables in the current schema
\dt

-- Describe the structure of a specific table (columns, types, indexes)
\d products

-- Show more detailed information about the table (including disk size)
\d+ products
```

---

## 4. 📝 Data Manipulation Language (DML) - Data Operations

These commands interact with the data stored inside the tables.

### A. Inserting Data

SQL

```bash
-- Insert a single row
INSERT INTO products (product_name, price) 
VALUES ('Mechanical Keyboard', 150.99);

-- Insert multiple rows
INSERT INTO products (product_name, price) 
VALUES ('Gaming Mouse', 45.00), ('Monitor Stand', 30.50);
```

### B. Retrieving Data (SELECT)

SQL

```bash
-- Select all columns and all rows from the table
SELECT * FROM products;

-- Select specific columns with a filtering condition
SELECT product_name, price 
FROM products 
WHERE price > 100.00
ORDER BY price DESC;
```

### C. Updating and Deleting Data

SQL

```bash
-- Update the price for a specific product
UPDATE products 
SET price = 125.00 
WHERE product_name = 'Mechanical Keyboard';

-- Remove a row based on a condition
DELETE FROM products 
WHERE id = 3;
```

---

## 5. 🛠️ Utilities and Advanced Operations

These commands are useful for scripting, exporting, and managing the session.

### A. Session Utilities

SQL

```bash
-- Toggle expanded display mode (useful for wide tables)
\x

-- Toggle display of query execution time
\timing

-- Execute commands from a SQL file
\i /path/to/my/script.sql

-- Execute a Linux shell command without leaving psql
\! ls -l
```

### B. Data Import/Export (`\copy`)

SQL

```bash
-- Import data from a client-side CSV file into the table
\copy products(product_name, price) FROM '~/data/new_items.csv' CSV HEADER;

-- Export the result of a query to a client-side CSV file
\copy (SELECT id, product_name, price FROM products WHERE price < 100) TO 'cheap_products.csv' CSV HEADER;
```