## Indexes & Query Performance

1. What is an Index?
An Index is a separate data structure the database maintains to find rows fast — without scanning every single row in the table. You create it on a column, and the database uses it automatically when that column appears in a WHERE, JOIN, or ORDER BY.

A database without indexes = a book without a table of contents. Every search reads every page.

1. Why Does It Matter?
A query on a 500-row table is fast regardless. But at 10 million rows:

Without index: the database reads every row — could take seconds
With index: it jumps directly to matching rows — milliseconds

## In production apps (e-commerce, fintech, SaaS), slow queries = slow pages = lost users. Indexes are the #1 performance tool every backend dev and data engineer must understand. They're also a common system design and SQL interview topic

1. The 20% That Covers 80% of Real Work
How to create an index
sql-- Basic index on a single column
CREATE INDEX idx_orders_user_id ON orders(user_id);

-- Index on multiple columns (composite index)
CREATE INDEX idx_orders_user_status ON orders(user_id, status);

-- Unique index — also enforces uniqueness like a constraint
CREATE UNIQUE INDEX idx_users_email ON users(email);

## How the database uses it — EXPLAIN

sql-- See the query execution plan before running
EXPLAIN SELECT * FROM orders WHERE user_id = 5;

Look for Seq Scan (bad — full table scan) vs Index Scan (good — using the index).
EXPLAIN ANALYZE runs the query and shows actual time taken.

When indexes help — and when they don't
-- ✅ Index on user_id HELPS — equality filter on indexed column
SELECT * FROM orders WHERE user_id = 5;

-- ✅ Index HELPS — JOIN on indexed column
SELECT * FROM orders o JOIN users u ON o.user_id = u.id;

-- ✅ Index HELPS — ORDER BY on indexed column
SELECT * FROM orders ORDER BY created_at DESC LIMIT 10;

-- ❌ Index IGNORED — function wrapped around column breaks index lookup
SELECT * FROM orders WHERE YEAR(created_at) = 2024;
-- Fix: WHERE created_at BETWEEN '2024-01-01' AND '2024-12-31'

-- ❌ Index IGNORED — leading wildcard in LIKE
SELECT * FROM users WHERE email LIKE '%@gmail.com';
-- Fix: use a full-text index, or reverse the pattern logic

##

Composite index — column order matters
sqlCREATE INDEX idx_orders_user_status ON orders(user_id, status);

-- ✅ Uses the index — matches leftmost column
SELECT * FROM orders WHERE user_id = 5;

-- ✅ Uses the index — matches both columns
SELECT * FROM orders WHERE user_id = 5 AND status = 'delivered';

-- ❌ Does NOT use the index — skipped the leftmost column
SELECT * FROM orders WHERE status = 'delivered';

Rule: a composite index works left to right. Always put the most selective or most-filtered column first.

## The cost of indexes — nothing is free

sql-- Every index slows down INSERT, UPDATE, DELETE
-- because the index must be updated too

-- Rule of thumb:
-- Read-heavy table (reports, analytics) → add indexes freely
-- Write-heavy table (logs, events, queues) → index sparingly

## 4. Real-Life Mental Model

ConceptAnalogyNo indexFinding a word by reading every page of a bookIndexUsing the book's index to jump to the right pageComposite indexIndex sorted by last name, then first name — searching by first name alone won't helpEXPLAINGPS showing the route before you driveIndex on write-heavy tableUpdating the index on every page edit — gets expensive fast
The 3 questions to ask before adding an index:

Is this column frequently used in WHERE, JOIN, or ORDER BY?
Does this table have enough rows that a full scan is actually slow?
Is this table read-heavy or write-heavy?

If yes, yes, read-heavy → add the index.

## Key Takeaway

Indexes are the first thing to reach for when a query is slow. The core insight: without an index, the database reads every row every time — at scale, that's the difference between 2ms and 2 seconds. Create indexes on columns you filter and join on, use EXPLAIN to verify they're being used, and never wrap indexed columns in functions.
