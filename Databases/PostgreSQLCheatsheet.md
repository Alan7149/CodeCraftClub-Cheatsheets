# 🐘 PostgreSQL Cheatsheet

> Part of [CodeCraftClub-Cheatsheets](../README.md) · PostgreSQL quick reference.

---

## psql CLI & Meta-Commands

```bash
psql -U postgres -d shop           # connect
```

```
\l            list databases
\c shop       connect to database
\dt           list tables
\d users      describe table
\du           list roles/users
\di           list indexes
\q            quit
\timing       toggle query timing
```

## Databases & Tables

```sql
CREATE DATABASE shop;
DROP DATABASE shop;

CREATE TABLE users (
  id         SERIAL PRIMARY KEY,        -- auto-increment
  name       VARCHAR(100) NOT NULL,
  email      TEXT UNIQUE,
  age        INTEGER DEFAULT 0,
  metadata   JSONB,                     -- native JSON
  tags       TEXT[],                    -- native array
  created_at TIMESTAMPTZ DEFAULT now()
);
```

## CRUD

```sql
INSERT INTO users (name, email) VALUES ('Alan', 'a@b.com')
RETURNING id;                            -- returns generated id

UPDATE users SET age = 22 WHERE id = 1;
DELETE FROM users WHERE id = 5;

SELECT * FROM users WHERE age >= 18 ORDER BY created_at DESC LIMIT 10;
SELECT * FROM users WHERE name ILIKE 'a%';   -- case-insensitive LIKE
```

## Upsert (ON CONFLICT)

```sql
INSERT INTO users (id, name, email)
VALUES (1, 'Alan', 'a@b.com')
ON CONFLICT (id)
DO UPDATE SET name = EXCLUDED.name;
```

## Joins & Aggregation

```sql
SELECT u.name, COUNT(o.id) AS orders, SUM(o.total) AS spent
FROM users u
LEFT JOIN orders o ON o.user_id = u.id
GROUP BY u.id, u.name
HAVING SUM(o.total) > 100
ORDER BY spent DESC;
```

## JSONB Queries (Postgres superpower)

```sql
SELECT metadata->>'theme' FROM users;          -- text value
SELECT metadata->'address'->>'city' FROM users;-- nested
SELECT * FROM users WHERE metadata @> '{"vip": true}';  -- contains
UPDATE users SET metadata = jsonb_set(metadata, '{theme}', '"dark"');
```

## Arrays

```sql
SELECT * FROM users WHERE 'admin' = ANY(tags);
SELECT * FROM users WHERE tags @> ARRAY['admin'];  -- contains
SELECT array_length(tags, 1) FROM users;
UPDATE users SET tags = array_append(tags, 'new');
```

## Window Functions

```sql
SELECT
  name,
  total,
  RANK()       OVER (ORDER BY total DESC)        AS rank,
  ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY created_at) AS rn,
  SUM(total)   OVER (PARTITION BY user_id)       AS user_total
FROM orders;
```

## CTEs (WITH clauses)

```sql
WITH big_spenders AS (
  SELECT user_id, SUM(total) AS spent
  FROM orders GROUP BY user_id HAVING SUM(total) > 1000
)
SELECT u.name, b.spent
FROM big_spenders b JOIN users u ON u.id = b.user_id;
```

## Indexes

```sql
CREATE INDEX idx_email ON users(email);
CREATE INDEX idx_meta ON users USING GIN (metadata);  -- for JSONB/arrays
CREATE UNIQUE INDEX idx_uniq ON users(email);
EXPLAIN ANALYZE SELECT * FROM users WHERE email = 'a@b.com';
```

## Transactions

```sql
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;        -- or ROLLBACK;

SAVEPOINT sp1; -- partial rollback point
ROLLBACK TO sp1;
```

## Handy Functions

```sql
SELECT now(), current_date, extract(year FROM created_at);
SELECT coalesce(email, 'none'), nullif(a, b);
SELECT generate_series(1, 5);          -- 1..5 rows
SELECT string_agg(name, ', ') FROM users;   -- concat group
```

---

[🔝 Back to README](../README.md)
