## Window Functions

1. What Are They?
A Window Function performs a calculation across a set of rows related to the current row — without collapsing them into one like GROUP BY does. You get the aggregated value and keep every individual row.

GROUP BY summarizes rows into one. Window functions add a new column alongside every row.

1. Why Does It Matter?
Some of the most common real-world data questions are impossible without window functions:

"Rank customers by total spend within each city"
"What was the revenue change from last month to this month?"
"Give each order a row number so I can pick the latest one per user"

## GROUP BY can't do this — it would lose the individual rows. Window functions are the #1 advanced SQL topic in data/backend interviews and are used constantly in analytics pipelines and dashboards

1. The 20% That Covers 80% of Real Work
The syntax blueprint
sqlfunction_name() OVER (
  PARTITION BY column   -- like GROUP BY, but keeps all rows
  ORDER BY column       -- defines order within each partition

## ROW_NUMBER — unique rank per row, no ties

sql-- Number each user's orders from oldest to newest
SELECT user_id, amount, created_at,
  ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY created_at ASC) AS rn
FROM orders;

Use to get the latest/first record per group — filter WHERE rn = 1 after wrapping in a CTE.

RANK — rank with gaps on ties
sql-- Rank orders by amount (highest = rank 1), ties get same rank
SELECT user_id, amount,
  RANK() OVER (ORDER BY amount DESC) AS rnk
FROM orders;

## If two rows tie at rank 2, the next rank is 4 (gap). Use DENSE_RANK() to avoid gaps

LAG & LEAD — look at previous / next row
sql-- Month-over-month revenue comparison
SELECT month, revenue,
  LAG(revenue, 1) OVER (ORDER BY month) AS prev_month_revenue,
  revenue - LAG(revenue, 1) OVER (ORDER BY month) AS change
FROM monthly_revenue;

LAG(col, n) = value n rows behind. LEAD(col, n) = value n rows ahead.
Use for trends, deltas, and sequential comparisons.

## SUM / AVG as window functions — running totals

sql-- Running total of revenue over time
SELECT created_at, amount,
  SUM(amount) OVER (ORDER BY created_at) AS running_total
FROM orders;

Without PARTITION BY, the window spans the whole table — perfect for running totals.

Real-World Pattern — get the latest order per user
sqlWITH ranked AS (
  SELECT *,
    ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY created_at DESC) AS rn
  FROM orders
)
SELECT user_id, amount, created_at
FROM ranked
WHERE rn = 1;

## The key difference to internalize

sql-- GROUP BY: collapses rows — you lose individual data
SELECT user_id, SUM(amount) FROM orders GROUP BY user_id;
-- Result: 1 row per user

-- Window function: adds a column — every row stays
SELECT user_id, amount, SUM(amount) OVER (PARTITION BY user_id) AS total
FROM orders;
-- Result: all rows, with each user's total alongside

## Key Takeaway

Window functions are what separate intermediate SQL from advanced SQL. The core insight: PARTITION BY is GROUP BY without losing rows, and ORDER BY inside OVER() defines the sequence for ranking and running calculations. Master ROW_NUMBER, LAG, and SUM OVER — and you can answer almost any time-series or ranking question cleanly.
