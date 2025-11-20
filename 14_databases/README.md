---
title: "Databases"
category: data-engineering
levels: ["mid", "senior", "principal"]
skills: [databases, sql, nosql, indexing]
questions:
  "mid": ["ACID vs BASE.", "SQL vs NoSQL.", "What is Normalization?"]
  "senior": ["Explain B-Tree vs LSM Tree.", "How does Database Sharding work?", "What is an Isolation Level?"]
  "principal": ["Design a distributed database (Spanner).", "Explain Write-Ahead Logging (WAL).", "Compare Row-oriented vs Column-oriented storage."]
---

# Databases

Databases are the persistent memory of any system. Understanding their internals is crucial for performance and reliability.

## Core Concepts

### 1. ACID Properties
*   **Atomicity**: All or nothing.
*   **Consistency**: DB moves from one valid state to another.
*   **Isolation**: Transactions don't interfere with each other.
*   **Durability**: Committed data is saved permanently (even if power fails).

### 2. SQL vs NoSQL
*   **SQL (Relational)**: Structured, Schema-on-write, Joins, ACID. (PostgreSQL, MySQL).
*   **NoSQL (Non-Relational)**: Unstructured, Schema-on-read, Horizontal Scale.
    *   **Key-Value**: Redis, DynamoDB.
    *   **Document**: MongoDB.
    *   **Column-Family**: Cassandra, HBase.
    *   **Graph**: Neo4j.

### 3. Indexing
*   **B-Tree**: Balanced Tree. Standard for SQL. Good for Read/Write. $O(\log N)$.
*   **LSM Tree (Log-Structured Merge)**: Optimized for Writes. Appends to memory, flushes to disk (SSTable). Used in Cassandra, RocksDB.
*   **Hash Index**: $O(1)$ lookup. No range queries.

## Advanced Topics

### 1. Isolation Levels
*   **Read Uncommitted**: Dirty reads allowed. Fastest, unsafe.
*   **Read Committed**: No dirty reads. Standard default.
*   **Repeatable Read**: No non-repeatable reads (Phantom reads possible).
*   **Serializable**: Strict execution. Slowest, safest.

### 2. Storage Engines
*   **Row-Oriented (OLTP)**: Stores data row by row. Good for transactions (INSERT/UPDATE single user). (Postgres).
*   **Column-Oriented (OLAP)**: Stores data column by column. Good for analytics (SUM(sales) over million rows). (Redshift, BigQuery, ClickHouse).

### 3. NewSQL
*   Attempts to combine the scalability of NoSQL with the ACID guarantees of SQL.
*   **Google Spanner**: Uses TrueTime (Atomic Clocks) to guarantee global consistency.
*   **CockroachDB**: Distributed SQL based on Raft.

## Interview Questions

### Mid Level
1.  **What is Normalization?**
    *   *Answer*: Organizing data to reduce redundancy and improve integrity.
    *   **1NF**: Atomic values.
    *   **2NF**: No partial dependencies.
    *   **3NF**: No transitive dependencies.
2.  **What is the difference between Clustered and Non-Clustered Index?**
    *   *Answer*:
        *   **Clustered**: Sorts the actual data rows on disk. Only one per table (usually Primary Key).
        *   **Non-Clustered**: Separate structure pointing to data rows. Can have many.
3.  **Explain the "N+1 Select" problem.**
    *   *Answer*: Fetching a list of N items (1 query), then running a separate query for each item to fetch related data (N queries). Total N+1. Fix: Use JOIN or `prefetch_related`.

### Senior Level
1.  **Why is LSM Tree better for writes than B-Tree?**
    *   *Answer*: B-Tree requires random disk I/O to update pages in place. LSM Tree writes sequentially (append-only) to a log/memtable, which is much faster on HDD/SSD.
2.  **Explain Optimistic vs Pessimistic Locking.**
    *   *Answer*:
        *   **Pessimistic**: Lock the record before updating. "I assume conflict will happen." Good for high contention.
        *   **Optimistic**: Read, check version, update if version hasn't changed. "I assume conflict won't happen." Good for low contention.
3.  **How does Postgres MVCC (Multi-Version Concurrency Control) work?**
    *   *Answer*: Writers don't block readers. Updates create a new version of the row (tuple) instead of overwriting. Old versions are kept for running transactions. Vacuum cleans up dead tuples.

### Principal Level
1.  **Explain Write-Ahead Logging (WAL).**
    *   *Discussion*: Before writing to the database file (which is random I/O and slow), changes are written to a sequential log (fast). If crash happens, replay the log to recover. Ensures Durability.
2.  **Design a Sharding Strategy for a Multi-Tenant SaaS.**
    *   *Discussion*:
        *   **Tenant ID Sharding**: All data for Tenant A on Shard 1. Good for isolation, bad for "Hot Tenant".
        *   **Entity ID Sharding**: Shard by UserID across all shards. Good for balance, complex queries (scatter-gather).
3.  **Compare CAP Theorem trade-offs in MongoDB vs Cassandra.**
    *   *Discussion*:
        *   **MongoDB**: CP (Consistency over Availability). Single Master. If Master dies, system is unavailable until election.
        *   **Cassandra**: AP (Availability over Consistency). Multi-Master. Writes always succeed, but might conflict (Eventual Consistency).
