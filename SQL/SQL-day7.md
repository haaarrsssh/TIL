## What Is This?

Six days of SQL — distilled into one reference sheet, followed by 10 practice problems that test everything together. This is your active recall day: no new syntax, just pattern recognition and problem solving.

Knowing syntax is reading a recipe. Solving problems is actually cooking.

1. The Full SQL Mental Map (Days 14–19)

DayTopicThe One Thing to Remember14SQL Basics5 commands: SELECT, WHERE, INSERT, UPDATE, DELETE15SELECT Deep DiveExecution order: FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT16JOINsLEFT JOIN keeps all left rows; INNER JOIN keeps only matches17Subqueries & CTEsCTE = named subquery; chain steps like a recipe18Window FunctionsOVER() keeps all rows; PARTITION BY is GROUP BY without collapsing19IndexesIndex on WHERE/JOIN columns; never wrap indexed columns in functions

1. 10 Practice Problems — Write the Query

Easy

P1. Get all users whose status is 'active', sorted by name A→Z.

P2. Count the total number of orders in the orders table.

P3. Find the most expensive order amount in the table.

P4. Get all distinct cities from the users table.

Medium

P5. Show each user's name and their total spend. Include users who have never ordered (show NULL or 0 for them).

P6. Find all users who have placed more than 3 orders.

P7. Show the top 3 cities by total revenue from orders.

P8. Get all orders where the amount is above the average order amount.

Hard

P9. For each user, get only their most recent order (one row per user).

P10. Show each order's amount alongside a running total of revenue, ordered by created_at.

1. Solutions

<details>
<summary>Click to reveal — try first!</summary>
sql-- P1
SELECT name FROM users
WHERE status = 'active'
ORDER BY name ASC;

-- P2
SELECT COUNT(*) FROM orders;

-- P3
SELECT MAX(amount) FROM orders;

-- P4
SELECT DISTINCT city FROM users;

-- P5
SELECT u.name, COALESCE(SUM(o.amount), 0) AS total_spent
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
GROUP BY u.name;

-- P6
SELECT user_id, COUNT(*) AS order_count
FROM orders
GROUP BY user_id
HAVING COUNT(*) > 3;

-- P7
SELECT u.city, SUM(o.amount) AS total_revenue
FROM users u
JOIN orders o ON u.id = o.user_id
GROUP BY u.city
ORDER BY total_revenue DESC
LIMIT 3;

-- P8
SELECT * FROM orders
WHERE amount > (SELECT AVG(amount) FROM orders);

-- P9
WITH ranked AS (
  SELECT *,
    ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY created_at DESC) AS rn
  FROM orders
)
SELECT user_id, amount, created_at
FROM ranked WHERE rn = 1;

-- P10
SELECT created_at, amount,
  SUM(amount) OVER (ORDER BY created_at) AS running_total
FROM orders;

</details>

1. Pattern → Problem Map (The Real 80/20)

If the problem says...Reach for..."for each user / per group"GROUP BY"include even if no match"LEFT JOIN"above/below the average"Subquery: WHERE col > (SELECT AVG...)"only groups where count > N"HAVING"latest / first record per user"CTE + ROW_NUMBER() OVER (PARTITION BY ... ORDER BY ...)"running total / cumulative"SUM() OVER (ORDER BY ...)"rank within a group"RANK() or DENSE_RANK() with PARTITION BY"change from previous row"LAG()

Key Takeaway

SQL mastery isn't about memorizing syntax — it's about recognizing which pattern fits which problem. The 8 patterns above cover 80% of real SQL interview questions and production queries. When stuck, translate the question to plain English, then map it to a pattern.
