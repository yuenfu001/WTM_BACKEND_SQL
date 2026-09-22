# MySQL Database Session & Key Lessons

**Environment:** MySQL Server 8.4.9 (Win64)  
**Database:** `wtm_backend`

---

## 1. Session Log & Analysis

### Database Setup & Operations
```sql
-- Dropping outdated databases
DROP DATABASE IF EXISTS wm_backend;
DROP DATABASE IF EXISTS birhane;

-- Creating and selecting target database
CREATE DATABASE wtm_backend;
USE wtm_backend;
```

---

### Table Creation & Alteration (`users`)

#### Creating the Base Table
```sql
CREATE TABLE users (
    username VARCHAR(40),
    firstname VARCHAR(70),
    lastname VARCHAR(70),
    password VARCHAR(15)
);
```

#### Altering Schema & Defining Primary Key
```sql
-- Add email column
ALTER TABLE users ADD COLUMN email VARCHAR(60);

-- Set email as Primary Key
ALTER TABLE users ADD CONSTRAINT pk_users PRIMARY KEY(email);
```

**Table Schema (`users`):**
| Field | Type | Null | Key | Default | Extra |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `username` | `varchar(40)` | YES | | NULL | |
| `firstname` | `varchar(70)` | YES | | NULL | |
| `lastname` | `varchar(70)` | YES | | NULL | |
| `password` | `varchar(15)` | YES | | NULL | |
| `email` | `varchar(60)` | NO | **PRI** | NULL | |

---

### Data Insertion & Updates (`users`)

```sql
-- Insert initial record
INSERT INTO users (username, firstname, lastname, password, email) 
VALUES ("raghad007", "Raghad", "Alkurdi", "yeay@1234", "raghad2026@gmail.com");

-- Attempt duplicate primary key (Fails with ERROR 1062)
-- INSERT INTO users (...) VALUES ("raghad007", "Raghad", "Alkurdi", "yeay@1234", "raghad2026@gmail.com");

-- Insert second user with unique email
INSERT INTO users (username, firstname, lastname, password, email) 
VALUES ("raghad007", "Raghad", "Alkurdi", "yeay@1234", "raghad2026@hotmail.com");

-- Updating record data
UPDATE users 
SET username = "raghad01", firstname = "Yeukai", lastname = "Marashe" 
WHERE email = "raghad2026@hotmail.com";

UPDATE users 
SET password = "yeukai@2026" 
WHERE email = "raghad2026@hotmail.com";
```

**Current Data (`users`):**
| username | firstname | lastname | password | email |
| :--- | :--- | :--- | :--- | :--- |
| `raghad007` | `Raghad` | `Alkurdi` | `yeay@1234` | `raghad2026@gmail.com` |
| `raghad01` | `Yeukai` | `Marashe` | `yeukai@2026` | `raghad2026@hotmail.com` |

---

### Child Table Creation (`userprofile`) with Foreign Key

```sql
CREATE TABLE userprofile (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id VARCHAR(40),
    country VARCHAR(50),
    city VARCHAR(60),
    address TEXT,
    FOREIGN KEY (user_id) REFERENCES users(email)
);
```

#### Inserting Profile Data
```sql
INSERT INTO userprofile (user_id, country, city, address) 
VALUES 
    ("raghad2026@gmail.com", "Malaysia", "Kuala", "11 something street, another province"),
    ("raghad2026@hotmail.com", "Uganda", "Kampala", "32nd vincent close, bob street, liu province");
```

---

### Enforcing 1-to-1 Relationship Constraint

To enforce that each user in `users` can have **at most one profile** in `userprofile`, a `UNIQUE` constraint must be applied to the foreign key column:

```sql
ALTER TABLE userprofile 
    ADD CONSTRAINT uq_user_id UNIQUE(user_id), 
    ADD CONSTRAINT fk_userp_users FOREIGN KEY(user_id) REFERENCES users(email);
```

**Final Schema (`userprofile`):**
| Field | Type | Null | Key | Default | Extra |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `id` | `int` | NO | **PRI** | NULL | `auto_increment` |
| `user_id` | `varchar(40)` | YES | **UNI** | NULL | |
| `country` | `varchar(50)` | YES | | NULL | |
| `city` | `varchar(60)` | YES | | NULL | |
| `address` | `text` | YES | | NULL | |

---

## 2. Common Errors Debugged During Session

1. **`ERROR 1046 (3D000): No database selected`**
   * **Cause:** Executed `CREATE TABLE` before running `USE wtm_backend;`.
   * **Fix:** Always select a target database using `USE <dbname>;` first.

2. **`ERROR 1062 (23000): Duplicate entry 'raghad2026@gmail.com' for key 'users.PRIMARY'`**
   * **Cause:** Attempted to insert a row with an email that already exists in the Primary Key column.
   * **Fix:** Ensure primary key values are strictly unique across all rows.

3. **`ERROR 1136 (21S01): Column count doesn't match value count at row 1`**
   * **Cause:** Missing a comma between values in the `INSERT` statement (`"raghad2026@gmail.com" "Malaysia"`).
   * **Fix:** Separate every column value with a comma.

4. **`ERROR 1064 (42000): You have an error in your SQL syntax...`**
   * **Cause:** Attempting `DELETE db_name;` instead of `DROP DATABASE db_name;`, or invalid constraint syntax (`ADD CONSTRAINT user_id UNIQUE FOREIGN KEY`).
   * **Fix:** Use `DROP DATABASE` for schemas, and specify constraint targets explicitly (`UNIQUE(column)`).