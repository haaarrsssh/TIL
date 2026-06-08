1. What Are They?

A Subquery is a query nested inside another query — an answer used as input for a bigger question.
A CTE (Common Table Expression) is a named, temporary result set you define once and reuse — like creating a mini table just for that query.

Subquery = query inside a query. CTE = give that inner query a name and breathe.

1. Why Does It Matter?
Some questions can't be answered in a single flat query. For example:

"Show me users who spent more than the average order value" — you need the average first, then compare
"Find the top 3 customers per city" — you need to rank within groups first, then filter

Subqueries and CTEs let you break complex problems into steps — exactly like how you'd think through the problem in plain English.
Real-world use: dashboards, reports, analytics pipelines, and almost every medium-to-hard SQL interview question.

1. The 20% That Covers 80% of Real Work
Subquery in WHERE — filter using a computed value
sql-- Users who placed orders above the average order amount
SELECT name FROM users
WHERE id IN (
  SELECT user_id FROM orders
  WHERE amount > (SELECT AVG(amount) FROM orders)
);

The innermost query runs first → gets the average → middle query finds matching user_ids → outer query gets their names.

Subquery in FROM — treat a query result as a table
sql-- Average spend per user, then filter those above ₹400
SELECT user_id, avg_spend
FROM (
  SELECT user_id, AVG(amount) AS avg_spend
  FROM orders
  GROUP BY user_id
) AS user_averages
WHERE avg_spend > 400;

## CTE — same thing, but readable

sql-- Same query rewritten as a CTE
WITH user_averages AS (
  SELECT user_id, AVG(amount) AS avg_spend
  FROM orders
  GROUP BY user_id
)
SELECT user_id, avg_spend
FROM user_averages
WHERE avg_spend > 400;

WITH name AS (...) defines the CTE. Everything after can reference it by name.
CTEs and subqueries produce the same result — CTEs are just easier to read and debug.

## Multiple CTEs — chain your logic like steps

sqlWITH active_users AS (
  SELECT id, name FROM users WHERE status = 'active'
),
user_totals AS (
  SELECT user_id, SUM(amount) AS total FROM orders GROUP BY user_id
)
SELECT a.name, t.total
FROM active_users a
JOIN user_totals t ON a.id = t.user_id
ORDER BY t.total DESC;

Each CTE builds on the previous. Reads like a recipe — step 1, step 2, final answer.

## 4. Real-Life Mental Model

ToolWhen to UseAnalogySubqueryQuick, one-off nested filter or valueA calculation inside a formulaCTEMulti-step logic, reuse, readabilityA named sticky note for laterMultiple CTEsComplex pipelines with 3+ stepsA recipe with numbered steps
The golden rule:

If your subquery appears more than once, or makes the query hard to read — convert it to a CTE.

## Execution mental model

WITH step1 AS (...)   -- runs first
   , step2 AS (...)   -- can use step1
SELECT ...            -- final answer uses step1 and/or step2
FROM step2

## Key Takeaway

Subqueries and CTEs are how you solve multi-step SQL problems without losing your mind. Subqueries are powerful but get messy when nested deep. CTEs give the same power with clarity — write your logic like a story, one named step at a time.
