# SESSION 1: MySQL Query Reference & Command Summary

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

# SESSION 2: MySQL Terminal Session Log

**Environment:** MySQL Server 8.4.9 (Win64)

**User Contexts:** `root@localhost`, `glory001@localhost`

## 1. Environment & Version Verification

```
PS C:\Users\liudo> mysql --version
C:\Program Files\MySQL\MySQL Server 8.4\bin\mysql.exe  Ver 8.4.9 for Win64 on x86_64 (MySQL Community Server - GPL)

```

## 2. Root Session: Database Creation & Access Management

### Connection Establishment

```
PS C:\Users\liudo> mysql -u root -p
Enter password: ****
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.4.9 MySQL Community Server - GPL

```

### Initial Database Inspection

```
mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| lms                |
| local_sales_db     |
| merlin_db          |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
7 rows in set (0.03 sec)

mysql> USE merlin_db;
Database changed

mysql> SHOW TABLES;
+---------------------+
| Tables_in_merlin_db |
+---------------------+
| userprofile         |
| users               |
+---------------------+
2 rows in set (0.08 sec)

```

### Database Initialization (`wtm_backend`)

```
mysql> CREATE DATABASE wtm_backend CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
Query OK, 1 row affected (0.04 sec)

mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| lms                |
| local_sales_db     |
| merlin_db          |
| mysql              |
| performance_schema |
| sys                |
| wtm_backend        |
+--------------------+
8 rows in set (0.01 sec)

mysql> USE wtm_backend;
Database changed

```

### User Creation and Privilege Granting

```
mysql> CREATE USER 'glory001'@'localhost' IDENTIFIED BY 'password01';
Query OK, 0 rows affected (0.02 sec)

mysql> GRANT ALL PRIVILEGES ON wtm_backend.* TO 'glory001'@'localhost';
Query OK, 0 rows affected (0.01 sec)

mysql> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.01 sec)

mysql> EXIT
Bye

```

## 3. Dedicated User Verification (`glory001`)

```
PS C:\Users\liudo> mysql -u glory001 -p
Enter password: **********
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 15
Server version: 8.4.9 MySQL Community Server - GPL

```

```
mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| performance_schema |
| wtm_backend        |
+--------------------+
3 rows in set (0.00 sec)

mysql> EXIT
Bye

```

## 4. Root Session: Querying & Relational Joins (`merlin_db`)

```
PS C:\Users\liudo> mysql -u root -p
Enter password: ****
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 16
Server version: 8.4.9 MySQL Community Server - GPL

```

```
mysql> USE merlin_db;
Database changed

mysql> SHOW TABLES;
+---------------------+
| Tables_in_merlin_db |
+---------------------+
| userprofile         |
| users               |
+---------------------+
2 rows in set (0.01 sec)

```

### Table Contents Inspection

#### `userprofile` Table

```
mysql> SELECT * FROM userprofile;
+-----+----------------------------+----------+--------------+-------------------------------------------------------------------------+
| id  | user_id                    | country  | city         | address                                                                 |
+-----+----------------------------+----------+--------------+-------------------------------------------------------------------------+
| 101 | stephaniesimon@hotmail.com | Malaysia | Kuala Lumpur | not as addressly as the other person                                    |
| 104 | fancoalioma2025@tesla.com  | Cameroon | Bambali      | No.11 something street, another thing close, dash avenue, this province |
+-----+----------------------------+----------+--------------+-------------------------------------------------------------------------+
2 rows in set (0.01 sec)

```

#### `users` Table

```
mysql> SELECT * FROM users;
+----------------------------+-----------+-----------+----------+------------------+
| email                      | username  | firstname | lastname | password         |
+----------------------------+-----------+-----------+----------+------------------+
| birhane2026@gmail.com      | birhane01 | Birhane   | Telayneh | niceperson1223   |
| fancoalioma2025@tesla.com  | franco01  | Stephanie | Simon    | alphanumeric@123 |
| stephaniesimon@hotmail.com | simon007  | Stephanie | Simon    | alphanumeric@123 |
+----------------------------+-----------+-----------+----------+------------------+
3 rows in set (0.01 sec)

```

### Inner Join Query Execution

```
mysql> SELECT * FROM users usr JOIN userprofile usp ON usr.email = usp.user_id;
+----------------------------+----------+-----------+----------+------------------+-----+----------------------------+----------+--------------+-------------------------------------------------------------------------+
| email                      | username | firstname | lastname | password         | id  | user_id                    | country  | city         | address                                                                 |
+----------------------------+----------+-----------+----------+------------------+-----+----------------------------+----------+--------------+-------------------------------------------------------------------------+
| fancoalioma2025@tesla.com  | franco01 | Stephanie | Simon    | alphanumeric@123 | 104 | fancoalioma2025@tesla.com  | Cameroon | Bambali      | No.11 something street, another thing close, dash avenue, this province |
| stephaniesimon@hotmail.com | simon007 | Stephanie | Simon    | alphanumeric@123 | 101 | stephaniesimon@hotmail.com | Malaysia | Kuala Lumpur | not as addressly as the other person                                    |
+----------------------------+----------+-----------+----------+------------------+-----+----------------------------+----------+--------------+-------------------------------------------------------------------------+
2 rows in set (0.00 sec)

```
