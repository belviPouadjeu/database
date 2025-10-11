## View (SQL)

This document explains SQL views: what they are, why to use them, common syntax for creation and replacement, examples, testing steps, and brief notes on performance and security. The examples use MySQL-compatible SQL and assume you have access to a database server.

### Table of contents

- Purpose
- Syntax
	- CREATE VIEW
	- CREATE OR REPLACE VIEW
- Examples
	- Simple view (single table)
	- View with join
	- Updatable view example
- Dropping a view
- Permissions and security
- Performance considerations
- Testing the view (MySQL CLI / PowerShell)
- Troubleshooting
- Related files in this repo

### Purpose

A view is a virtual table defined by a SELECT query. It provides:

- A simplified interface for complex queries
- A way to restrict access to columns or rows (security/abstraction)
- Re-usable query logic

Views do not store data themselves (unless the DBMS supports materialized views). They present the result of the underlying SELECT when queried.

### Syntax

CREATE VIEW (basic)

```sql
CREATE VIEW view_name AS
SELECT column1, column2, ...
FROM table_name
WHERE condition;
```

CREATE OR REPLACE VIEW (MySQL 8.0+ / compatible)

```sql
CREATE OR REPLACE VIEW view_name AS
SELECT ...;
```

Notes:

- Some databases (older MySQL versions) may not support CREATE OR REPLACE VIEW. Check your server version.
- The owning user and privileges determine whether a view can be created or replaced.

### Examples

1) Simple view (single table)

```sql
-- Create a view that exposes only id, name from a users table
CREATE VIEW v_users_basic AS
SELECT id, name
FROM users
WHERE active = 1;

-- Query the view
SELECT * FROM v_users_basic;
```

2) View with JOIN

```sql
CREATE VIEW v_order_summary AS
SELECT o.id AS order_id,
			 o.created_at,
			 c.name   AS customer_name,
			 SUM(oi.qty * oi.price) AS order_total
FROM orders o
JOIN customers c ON c.id = o.customer_id
JOIN order_items oi ON oi.order_id = o.id
GROUP BY o.id, o.created_at, c.name;

SELECT * FROM v_order_summary WHERE order_total > 100;
```

3) Updatable view (single base table, simple projection)

```sql
-- If a view references a single table and is a direct projection, many engines allow updates
CREATE VIEW v_products_editable AS
SELECT id, name, price
FROM products;

-- Update through the view (propagates to products table)
UPDATE v_products_editable
SET price = price * 1.05
WHERE id = 42;
```

If the view contains aggregates, DISTINCT, GROUP BY, or joins, it's typically not updatable.

### Dropping a view

```sql
DROP VIEW IF EXISTS view_name;
```

Use IF EXISTS to avoid an error when the view does not exist.

### Permissions and security

- To create or drop a view you need appropriate privileges (CREATE VIEW, DROP). The exact privilege names vary by RDBMS.
- Views can be used to limit column or row visibility for less-privileged users (GRANT SELECT ON view_name TO 'appuser'@'%').

### Performance considerations

- Views are query definitions; they don't generally improve query performance unless the DBMS supports materialized views or the optimizer rewrites queries.
- Complex views (many joins/subqueries) can be slower when used in additional queries. Test plan and EXPLAIN any heavy queries.

### Testing the view (MySQL CLI / PowerShell)

From PowerShell, using the MySQL CLI client (example):

```powershell
# connect then run a query
mysql -u root -p -e "SELECT * FROM v_users_basic LIMIT 5;" my_database
```

Or start the interactive client:

```powershell
mysql -u root -p my_database
-- then in the client:
SELECT * FROM v_users_basic LIMIT 10;
```

### Troubleshooting

- Error: "View's SELECT contains a subquery in the FROM clause" — some engines restrict CREATE VIEW; rewrite query or use a temporary table.
- Error: "PROCEDURE ... doesn't exist" when creating with functions — ensure deterministic/privileged functions are available and allowed in views.
- Unexpected NULLs — verify joins and WHERE conditions in the underlying SELECT.

### Related files in this repo

- `hr.sql` — HR schema examples (check for tables that can be used to create sample views)
- `mysql_learners.sql` — learner-oriented MySQL examples
- `PETRESCUE-CREATE.sql` — schema for the PETRESCUE project

### Notes

- Adjust SQL examples to match your schema names in this repository. If you want, I can add concrete CREATE VIEW examples using tables from `PETRESCUE-CREATE.sql` or `hr.sql` — tell me which file to read and I'll extract table/column names and produce ready-to-run statements.

---
