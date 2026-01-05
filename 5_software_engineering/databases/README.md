---
title: "Databases & Data Engineering for AI"
category: must
levels: ["mid", "senior", "principal"]
skills: [vector-dbs, feature-stores, data-lakes, sql, etl]
questions:
  "mid": ["What is the difference between a Vector Database and a standard Relation Database?"]
  "senior": ["Explain the concept of HNSW (Hierarchical Navigable Small World) indexing."]
  "principal": ["Design a data architecture for real-time personalization that combines batch user profiles with streaming click events."]
---

# Databases & Data Engineering for AI

## 💡 Core Ideas

Data is the fuel for AI. Senior Engineers must understand how to store, clean, and serve data efficiently.

*   **Vector Databases**: Storing and retrieving high-dimensional embeddings.
    *   **Algorithms**: HNSW, IVFPQ (Inverted File with Product Quantization).
    *   **Tools**: Pinecone, Milvus, Weaviate, pgvector.
*   **Feature Stores**: The bridge between data engineering and ML.
    *   **Concepts**: Point-in-time correctness, Online/Offline consistency.
    *   **Tools**: Feast, Tecton.
*   **Data Formats**:
    *   **Training Optimized**: Parquet, Avro, TFRecord, WebDataset (for sharded large-scale data).
*   **Data Lakes & Warehouses**: Snowflake, BigQuery, Databricks (Lakehouse).


## Types:
- Relational Databases (PostgreSQL, MySQL).
- NoSQL (MongoDB, Cassandra, DynamoDB).
- Vector Databases (Pinecone, Milvus).
- Time Series Databases (InfluxDB).
- Graph Databases (Neo4j).
- NewSQL (CockroachDB).

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

### 4. ACID Properties
*   **Atomicity**: All operations succeed or none do.
*   **Consistency**: Data must be in a valid state.
*   **Isolation**: Transactions are isolated from each other.
*   **Durability**: Changes are permanent.

### 5. Indexing
*   **Primary Key**: Unique identifier for rows.
*   **Secondary Index**: Non-unique index for faster lookups.
*   **Composite Index**: Index on multiple columns.

### 6. Transactions
*   **ACID Properties**: Atomicity, Consistency, Isolation, Durability.
*   **Isolation Levels**: Read Uncommitted, Read Committed, Repeatable Read, Serializable.
*   **Locking**: Prevents concurrent access to data.

### 7. Sharding
*   **Horizontal Scaling**: Distributing data across multiple servers.
*   **Shard Key**: Column used to distribute data.
*   **Replication**: Copying data to multiple servers.

### 8. Partitioning
*   **Vertical Scaling**: Increasing resources on a single server.
*   **Horizontal Scaling**: Distributing data across multiple servers.
*   **Shard Key**: Column used to distribute data.
*   **Replication**: Copying data to multiple servers.

### 9. Data Models
*   **Relational**: Tables with rows and columns.
*   **Document**: JSON-like documents.
*   **Key-Value**: Simple key-value pairs.
*   **Columnar**: Columns of data.
*   **Graph**: Nodes and edges.

### 10. Data Formats
*   **CSV**: Comma-separated values.
*   **JSON**: JavaScript Object Notation.
*   **Parquet**: Columnar storage format.
*   **Avro**: Self-describing data format.
*   **ORC**: Optimized Row Columnar format.

### 11. Data Storage
*   **Data Warehouse**: Centralized repository for data.
*   **Data Lake**: Raw data repository.

### 📚 Resources

*   [**Designing Data-Intensive Applications**](https://dataintensive.net/) - The classic book by Martin Kleppmann.
*   [**Nearest Neighbor Search**](https://github.com/erikbern/ann-benchmarks) - Benchmarks for ANN algorithms.
*   [**Feature Stores for ML**](https://www.featurestore.org/) - Community resource.
*   [Database Internals](https://www.oreilly.com/library/view/database-internals/9781492040330/)

## 🛠️ Practice

### Project 1: RAG System with Vector DB
Build a Retrieval-Augmented Generation system.
*   **Goal**: Ingest a PDF, chunk it, embed chunks (OpenAI/HuggingFace), store in a local Vector DB (Chroma/FAISS), and query it.

### Project 2: Feature Store Implementation
Use `Feast` to define features for a fraud detection model.
*   **Goal**: Generate training data (offline) and serve feature vectors (online) via API key.

## 🗣️ Interview Questions

### Mid-Level
*   **Q**: Why do we use Parquet files for training data instead of CSV?
*   **A**: Parquet is columnar (faster read for specific columns), supports compression per column, and retains schema information / data types, whereas CSV is row-based, text-only, and slow to parse.

### Senior-Level
*   **Q**: How does Product Quantization (PQ) reduce memory usage in Vector DBs?
*   **A**: PQ splits vectors into sub-vectors and clusters them separately. Instead of storing the full float vectors, it stores the cluster ID (code) for each sub-vector, compressing high-dimensional data significantly (e.g., 32x reduction).
*   **Q**: What is Normalization?**
    *   *Answer*: Organizing data to reduce redundancy and improve integrity.
    *   **1NF**: Atomic values.
    *   **2NF**: No partial dependencies.
    *   **3NF**: No transitive dependencies.
*   **Q**: What is the difference between Clustered and Non-Clustered Index?**
    *   *Answer*:
        *   **Clustered**: Sorts the actual data rows on disk. Only one per table (usually Primary Key).
        *   **Non-Clustered**: Separate structure pointing to data rows. Can have many.
*   **Q**: Explain the "N+1 Select" problem.**
    *   *Answer*: Fetching a list of N items (1 query), then running a separate query for each item to fetch related data (N queries). Total N+1. Fix: Use JOIN or `prefetch_related`.

### Principal-Level
*   **Q**: Design a system to deduplicate 1 Petabyte of image data.
*   **A**:
    1.  **Compute hashes**: Use perceptual hashes (pHash) or embeddings (CLIP) for semantic deduplication.
    2.  **Distributed Processing**: Use Spark/Ray map-reduce job to compute hashes.
    3.  **Similarity Search**: LSH (Locality Sensitive Hashing) or Faiss on the hashes to find near-duplicates.
    4.  **Graph Connected Components**: Treat images as nodes, similarities as edges, and keep only one image per connected component.
*   **Q**: Why is LSM Tree better for writes than B-Tree?**
    *   *Answer*: B-Tree requires random disk I/O to update pages in place. LSM Tree writes sequentially (append-only) to a log/memtable, which is much faster on HDD/SSD.
*   **Q**: Explain Optimistic vs Pessimistic Locking.**
    *   *Answer*:
        *   **Pessimistic**: Lock the record before updating. "I assume conflict will happen." Good for high contention.
        *   **Optimistic**: Read, check version, update if version hasn't changed. "I assume conflict won't happen." Good for low contention.
3.  **How does Postgres MVCC (Multi-Version Concurrency Control) work?**
    *   *Answer*: Writers don't block readers. Updates create a new version of the row (tuple) instead of overwriting. Old versions are kept for running transactions. Vacuum cleans up dead tuples.
4.  **Explain Write-Ahead Logging (WAL).**
    *   *Discussion*: Before writing to the database file (which is random I/O and slow), changes are written to a sequential log (fast). If crash happens, replay the log to recover. Ensures Durability.
5.  **Design a Sharding Strategy for a Multi-Tenant SaaS.**
    *   *Discussion*:
        *   **Tenant ID Sharding**: All data for Tenant A on Shard 1. Good for isolation, bad for "Hot Tenant".
        *   **Entity ID Sharding**: Shard by UserID across all shards. Good for balance, complex queries (scatter-gather).
*   **Q**: Compare CAP Theorem trade-offs in MongoDB vs Cassandra.**
    *   *Answer*:
        *   **MongoDB**: CP (Consistency over Availability). Single Master. If Master dies, system is unavailable until election.
        *   **Cassandra**: AP (Availability over Consistency). Multi-Master. Writes always succeed, but might conflict (Eventual Consistency).
