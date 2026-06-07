## 1. What is a JOIN?

A JOIN combines rows from two or more tables based on a related column between them. Instead of storing everything in one giant table, databases split data into smaller, focused tables — JOINs are how you stitch them back together when you need to ask questions across both.

JOIN = the bridge between related tables.

## 2. Why Does It Matter?

Real databases never store everything in one table. A typical app has:

users table — who you are
orders table — what you bought
products table — what exists in the store

## To answer "What did Harsh order, and how much did he spend?" — you need a JOIN. This is the #1 most-tested SQL concept in interviews and the most used in real backend/data work

1. The 20% of JOINs That Cover 80% of Real Work
Setup — two simple tables:

## INNER JOIN — only matching rows from both tables

sqlSELECT users.name, orders.amount
FROM users
INNER JOIN orders ON users.id = orders.user_id;
Result: Harsh (500), Harsh (300), Riya (700)

Arjun is excluded (no orders). The orphan order (user_id=99) is excluded too.
Use when: you only care about data that exists on both sides.

LEFT JOIN — all rows from left table, matched rows from right
SELECT users.name, orders.amount
FROM users
LEFT JOIN orders ON users.id = orders.user_id;

## Result: Harsh (500), Harsh (300), Riya (700), Arjun (NULL)

Arjun appears with NULL — he has no orders but still shows up.
Use when: you want everyone, even those with no related data.

RIGHT JOIN — all rows from right table, matched rows from left
sqlSELECT users.name, orders.amount
FROM users
RIGHT JOIN orders ON users.id = orders.user_id;

Result: Harsh (500), Harsh (300), Riya (700), NULL (200)

The orphan order shows up with NULL for name.
Use when: you want all records from the right table, even unmatched ones.
(In practice, most people rewrite this as a LEFT JOIN by swapping table order.)

1. Real-Life Mental Model
JOIN TypeVenn Diagram ViewReal QuestionINNER JOINOnly the overlapping center"Show me users who have placed orders"LEFT JOINAll of left + overlap"Show me all users, with their orders if any"RIGHT JOINAll of right + overlap"Show me all orders, with user info if any"
The one rule to remember:

LEFT JOIN is the most used JOIN in real work. When in doubt, use LEFT JOIN — it never drops rows from your primary table.

## Key Takeaway

JOINs are what make relational databases relational. Master INNER JOIN and LEFT JOIN and you'll handle 90% of real-world multi-table queries. The key insight: data lives in separate tables by design — JOINs are how you ask questions that span all of them.
