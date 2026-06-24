# 🐬 MySQL Cheatsheet

> Part of [CodeCraftClub-Cheatsheets](../README.md) · MySQL / SQL quick reference.

---

## Connect & Admin

```bash
mysql -u root -p                 # connect
```

```sql
SHOW DATABASES;
CREATE DATABASE shop;
USE shop;
SHOW TABLES;
DESCRIBE users;                  -- or: DESC users;
DROP DATABASE shop;
```

## Create Tables

```sql
CREATE TABLE users (
  id        INT AUTO_INCREMENT PRIMARY KEY,
  name      VARCHAR(100) NOT NULL,
  email     VARCHAR(100) UNIQUE,
  age       INT DEFAULT 0,
  active    BOOLEAN DEFAULT TRUE,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE orders (
  id      INT AUTO_INCREMENT PRIMARY KEY,
  user_id INT,
  total   DECIMAL(10, 2),
  FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);
```

## Insert / Update / Delete

```sql
INSERT INTO users (name, email, age) VALUES ('Alan', 'a@b.com', 21);
INSERT INTO users (name, email) VALUES ('Bob', 'bob@b.com'), ('Cara', 'c@b.com');

UPDATE users SET age = 22 WHERE id = 1;
UPDATE users SET active = FALSE WHERE age < 18;

DELETE FROM users WHERE id = 5;
TRUNCATE TABLE users;            -- delete all rows, reset
```

## Select & Filter

```sql
SELECT * FROM users;
SELECT name, email FROM users;
SELECT * FROM users WHERE age >= 18;
SELECT * FROM users WHERE name LIKE 'A%';      -- starts with A
SELECT * FROM users WHERE age BETWEEN 18 AND 30;
SELECT * FROM users WHERE id IN (1, 2, 3);
SELECT * FROM users WHERE email IS NOT NULL;
SELECT DISTINCT age FROM users;

SELECT * FROM users
  ORDER BY age DESC, name ASC
  LIMIT 10 OFFSET 20;
```

## Aggregation & Grouping

```sql
SELECT COUNT(*) FROM users;
SELECT AVG(age), MAX(age), MIN(age), SUM(total) FROM orders;

SELECT user_id, COUNT(*) AS order_count, SUM(total) AS spent
FROM orders
GROUP BY user_id
HAVING SUM(total) > 100              -- filter on aggregates
ORDER BY spent DESC;
```

## Joins

```sql
-- INNER: only matching rows
SELECT u.name, o.total
FROM users u
INNER JOIN orders o ON o.user_id = u.id;

-- LEFT: all users, even with no orders
SELECT u.name, o.total
FROM users u
LEFT JOIN orders o ON o.user_id = u.id;

-- RIGHT and CROSS JOIN also exist
```

## Subqueries

```sql
SELECT name FROM users
WHERE id IN (SELECT user_id FROM orders WHERE total > 100);

SELECT name, (SELECT COUNT(*) FROM orders o WHERE o.user_id = u.id) AS cnt
FROM users u;
```

## Indexes & Constraints

```sql
CREATE INDEX idx_email ON users(email);
CREATE UNIQUE INDEX idx_uniq ON users(email);
ALTER TABLE users ADD COLUMN phone VARCHAR(20);
ALTER TABLE users DROP COLUMN phone;
ALTER TABLE users MODIFY age SMALLINT;
SHOW INDEX FROM users;
```

## Transactions

```sql
START TRANSACTION;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;       -- or ROLLBACK; to undo
```

## Useful Functions

```sql
SELECT NOW(), CURDATE(), YEAR(created_at);
SELECT UPPER(name), LOWER(email), LENGTH(name);
SELECT CONCAT(name, ' <', email, '>');
SELECT COALESCE(phone, 'none');         -- first non-null
SELECT CASE WHEN age >= 18 THEN 'adult' ELSE 'minor' END FROM users;
```

---

[🔝 Back to README](../README.md)
