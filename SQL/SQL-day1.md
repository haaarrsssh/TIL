1. What is SQL?
SQL (Structured Query Language) is the standard language for talking to relational databases. You use it to store, retrieve, update, and delete data that lives in tables (rows + columns).

Think of it as Excel — but for millions of rows, with superpowers.

1. Why Does It Matter?
Almost every real-world app stores data somewhere:

Instagram stores your posts, likes, and followers
Zomato stores your orders and restaurant menus
Your bank stores every transaction you've ever made

All of that lives in a database. SQL is how developers (and analysts) ask questions like "show me all orders above ₹500 placed this week" — in seconds, across millions of records.
Bottom line: If you work with data at any level, SQL is non-negotiable.

1. The 20% of SQL That Covers 80% of Real Work
These 5 commands will handle the vast majority of what you'll ever need:
-- 1. SELECT — read data
SELECT name, email FROM users;

-- 2. WHERE — filter rows
SELECT * FROM orders WHERE amount > 500;

-- 3. INSERT — add new data
INSERT INTO users (name, email) VALUES ('Harsh', '<harsh@example.com>');

-- 4. UPDATE — change existing data
UPDATE users SET email = '<new@example.com>' WHERE name = 'Harsh';

-- 5. DELETE — remove data
DELETE FROM orders WHERE status = 'cancelled';
-- JOIN — combine two related tables
SELECT users.name, orders.amount
FROM users
JOIN orders ON users.id = orders.user_id;
4. Real-Life Mental Model
SQL CommandReal-Life EquivalentSELECTSearch / filter in a spreadsheetWHEREApply a filter conditionINSERTFill a new row in a formUPDATEEdit an existing cellDELETEDelete a rowJOINMerge two sheets by a common column

## Key Takeaway

SQL isn't just a database tool — it's how the entire web reads and writes its memory. Learning the 5 core commands gives you 80% of the capability used in real jobs, real projects, and real interviews.
