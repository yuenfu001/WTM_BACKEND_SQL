# MySQL Query Reference & Command Summary

A quick-reference guide summarizing the valid SQL commands and queries executed during the terminal sessions, categorized by their administrative, structural, or data manipulation functions.

---

## 1. Environment & Database Administration

| Command / Query | Description |
| :--- | :--- |
| `SHOW DATABASES;` | Lists all databases available on the current MySQL server instance. |
| `CREATE DATABASE merlin_db;` | Creates a new database named `merlin_db`. |
| `USE merlin_db;` | Selects `merlin_db` as the active database context for subsequent queries. |
| `DROP DATABASE wtm_backend;` | Permanently deletes the `wtm_backend` database and all its contained tables. |

---

## 2. Table Inspection & Schema Definition (DDL)

| Command / Query | Description |
| :--- | :--- |
| `SHOW TABLES;` | Displays all tables within the currently selected database. |
| `SHOW COLUMNS FROM users;` | Displays column names, data types, nullability, keys, and default values for the `users` table. |
| `CREATE TABLE users (email VARCHAR(60), username VARCHAR(30), firstname VARCHAR(100), lastname VARCHAR(100), password VARCHAR(20));` | Creates the basic `users` table structure with five string columns. |
| `ALTER TABLE users ADD COLUMN email VARCHAR(60);` | Adds a new column named `email` to an existing table. |
| `ALTER TABLE users ADD CONSTRAINT pk_users PRIMARY KEY(email);` | Sets `email` as the Primary Key for the `users` table. |
| `ALTER TABLE users ADD CONSTRAINT UNIQUE(username);` | Enforces that all values in the `username` column must be unique across the table. |
| `CREATE TABLE userprofile (id INT AUTO_INCREMENT PRIMARY KEY, user_id VARCHAR(60) UNIQUE, country VARCHAR(30), city VARCHAR(30), address TEXT, FOREIGN KEY(user_id) REFERENCES users(email)) AUTO_INCREMENT=101;` | Creates the `userprofile` table with a One-to-One Foreign Key relationship pointing to `users(email)`, initializing auto-increment IDs starting at `101`. |

---

## 3. Data Manipulation Language (DML)

### Insertion (`INSERT`)

| Command / Query | Description |
| :--- | :--- |
| `INSERT INTO users(email, username, firstname, lastname, password) VALUES("stephaniesimon@hotmail.com", "simon007", "Stephanie", "Simon", "alphanumeric@123");` | Inserts a single record into the `users` table. |
| `INSERT INTO userprofile(user_id, country, city, address) VALUES ("stephaniesimon@hotmail.com", "Malaysia", "Kuala Lumpur", "not as addressly as the other person"), ("birhane2026@gmail.com", "Ethiopia", "Addis Ababa", NULL);` | Bulk inserts multiple profile records in a single query execution. |

### Querying (`SELECT`)

| Command / Query | Description |
| :--- | :--- |
| `SELECT * FROM users;` | Retrieves all rows and columns from the `users` table. |
| `SELECT email, username, firstname, lastname, password FROM users;` | Retrieves only specified columns from the `users` table. |
| `SELECT * FROM userprofile WHERE address IS NULL;` | Filters and retrieves records where the `address` field contains a `NULL` value. |
| `SELECT * FROM userprofile WHERE address IS NOT NULL;` | Filters and retrieves records where the `address` field contains data (excluding `NULL`). |

### Updating (`UPDATE`)

| Command / Query | Description |
| :--- | :--- |
| `UPDATE users SET username="birhane01" WHERE email="stephaniesimon@yahoo.com";` | Updates the `username` column for the record matching the specific `email`. |
| `UPDATE users SET firstname="Birhane", lastname="Telayneh", password="niceperson1223" WHERE email="stephaniesimon@yahoo.com";` | Updates multiple fields simultaneously for a target user record. |
| `UPDATE userprofile SET country="Ethiopia", city="Addis Ababa", address=NULL;` | **Global update:** Modifies values across *all* rows in the `userprofile` table (no `WHERE` clause). |

### Deletion (`DELETE`)

| Command / Query | Description |
| :--- | :--- |
| `DELETE FROM userprofile WHERE address IS NULL;` | Deletes specific rows from `userprofile` where the `address` value is `NULL`. |
