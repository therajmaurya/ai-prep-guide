---
title: "SQL for Data Science"
category: coding
levels: ["mid", "senior", "principal"]
skills: [sql, database-design, performance-optimization, data-modeling]
questions:
  "mid": ["Difference between WHERE and HAVING.", "Explain Types of Joins.", "What is a Primary Key vs Foreign Key?"]
  "senior": ["Explain Window Functions (RANK vs DENSE_RANK).", "How to optimize a slow query?", "What are CTEs?"]
  "principal": ["Design a schema for a high-throughput event logging system.", "Explain ACID properties in detail.", "Discuss Indexing strategies (B-Tree vs Hash)."]
---

# SQL for Data Science & ML

SQL is the universal language of data. For AI/ML roles, you need to be able to write complex queries to extract, clean, and aggregate data for training. Performance optimization is also key when dealing with petabyte-scale data warehouses (BigQuery, Redshift, Snowflake).

## Core Concepts

### 1. Joins
*   **INNER JOIN**: Returns records that have matching values in both tables.
*   **LEFT (OUTER) JOIN**: Returns all records from the left table, and the matched records from the right table.
*   **RIGHT (OUTER) JOIN**: Returns all records from the right table, and the matched records from the left table.
*   **FULL (OUTER) JOIN**: Returns all records when there is a match in either left or right table.
*   **CROSS JOIN**: Cartesian product (all combinations).

### 2. Aggregation & Filtering
*   **GROUP BY**: Groups rows that have the same values into summary rows.
*   **HAVING**: Filters groups *after* aggregation (unlike `WHERE` which filters rows *before* aggregation).
    ```sql
    SELECT department, COUNT(*)
    FROM employees
    WHERE salary > 50000
    GROUP BY department
    HAVING COUNT(*) > 10;
    ```

### 3. Window Functions
Perform calculations across a set of table rows that are somehow related to the current row. Unlike `GROUP BY`, they do not collapse rows.
*   **Syntax**: `FUNCTION() OVER (PARTITION BY col1 ORDER BY col2)`
*   **Ranking**:
    *   `ROW_NUMBER()`: Unique sequential number (1, 2, 3, 4).
    *   `RANK()`: Gaps for ties (1, 2, 2, 4).
    *   `DENSE_RANK()`: No gaps for ties (1, 2, 2, 3).
*   **Aggregation**: `SUM()`, `AVG()`, `MAX()` as window functions (running totals).
*   **Offset**: `LAG()` (previous row), `LEAD()` (next row).

### 4. Common Table Expressions (CTEs)
Temporary result sets defined within the execution scope of a single statement. Makes complex queries readable.
```sql
WITH RegionalSales AS (
    SELECT region, SUM(amount) as total_sales
    FROM orders
    GROUP BY region
)
SELECT * FROM RegionalSales WHERE total_sales > 100000;
```

## Advanced Topics

### 1. Performance Optimization
*   **EXPLAIN / EXPLAIN ANALYZE**: Shows the query execution plan (scans, joins, costs).
*   **Indexing**:
    *   **B-Tree**: Default. Good for range queries (`<`, `>`, `=`).
    *   **Hash**: Good for equality checks (`=`). O(1) lookup.
    *   **Clustered Index**: Sorts the actual data rows on disk. Only one per table.
*   **Partitioning**: Splitting a large table into smaller pieces (e.g., by date) to speed up queries.

### 2. SQL for AI (Vector Search)
Modern SQL databases (PostgreSQL) now support vector search.
*   **`pgvector`**: Extension for storing embeddings.
*   **Distance Metrics**: L2 (Euclidean), Cosine Similarity, Inner Product.
*   **Indexing**: IVFFlat or HNSW (Hierarchical Navigable Small Worlds) for Approximate Nearest Neighbor (ANN) search inside a standard SQL DB.
    ```sql
    SELECT * FROM item_embeddings 
    ORDER BY embedding <=> '[0.1, 0.3, ...]' 
    LIMIT 5;
    ```

### 2. ACID Properties
Transactions must be ACID to ensure data integrity.
*   **Atomicity**: All or nothing.
*   **Consistency**: Database remains in a valid state.
*   **Isolation**: Concurrent transactions don't interfere.
*   **Durability**: Committed transactions remain committed even after crash.

### 3. Isolation Levels
Trade-off between consistency and performance.
*   **Read Uncommitted**: Dirty reads allowed. Fastest.
*   **Read Committed**: No dirty reads. (Default in Postgres).
*   **Repeatable Read**: No non-repeatable reads (phantom reads possible).
*   **Serializable**: Strict execution. Slowest.

## Interview Questions

### Mid Level
1.  **What is the difference between `UNION` and `UNION ALL`?**
    *   *Answer*: `UNION` removes duplicate records. `UNION ALL` keeps duplicates and is faster (no sorting/deduplication step).
2.  **How do you find the 2nd highest salary?**
    *   *Answer*:
        ```sql
        SELECT MAX(salary) FROM employees
        WHERE salary < (SELECT MAX(salary) FROM employees);
        -- OR using Window Function
        SELECT salary FROM (
            SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) as rnk
            FROM employees
        ) WHERE rnk = 2;
        ```
3.  **What is a Self Join?**
    *   *Answer*: Joining a table with itself. Useful for hierarchical data (e.g., finding an employee's manager in the same table).

### Senior Level
1.  **Explain the difference between `RANK`, `DENSE_RANK`, and `ROW_NUMBER`.**
    *   *Answer*: `ROW_NUMBER` gives unique ID. `RANK` skips numbers for ties (1, 1, 3). `DENSE_RANK` does not skip (1, 1, 2).
2.  **How does a B-Tree index work?**
    *   *Answer*: Balanced tree structure. Keeps data sorted. Allows logarithmic time search ($O(\log N)$) for equality and range queries.
3.  **What is a "correlated subquery"?**
    *   *Answer*: A subquery that uses values from the outer query. It is executed once for *each row* processed by the outer query, making it very slow. Avoid if possible (rewrite as JOIN).

### Principal Level
1.  **Design a database schema for a social media feed.**
    *   *Discussion*: Discuss normalization (3NF) vs denormalization (for read performance). Sharding strategy (by UserID). Caching layer (Redis).
2.  **How would you optimize a query that performs a full table scan on a 10TB table?**
    *   *Discussion*: Check if an index can be added. Check if partition pruning is working (add `WHERE date = ...`). Use column-oriented storage (Parquet/BigQuery) to read only necessary columns. Use sampling.
3.  **Explain "Phantom Reads" and how to prevent them.**
    *   *Discussion*: A phantom read occurs when a transaction reads a set of rows that satisfy a condition, but a second transaction inserts a new row that matches the condition before the first transaction finishes. Prevented by `SERIALIZABLE` isolation level or Snapshot Isolation.
