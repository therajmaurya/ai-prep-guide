---
title: "High Level Design (HLD)"
category: system-design
levels: ["senior", "principal"]
skills: [system-design, scalability, architecture, distributed-systems]
questions:
  "senior": ["Explain CAP Theorem.", "Load Balancing algorithms.", "SQL vs NoSQL trade-offs."]
  "principal": ["Design a Global Video Streaming Service (Netflix).", "How to handle 10M concurrent connections?", "Discuss Consistency patterns (Strong vs Eventual)."]
---

# High Level Design (HLD)

HLD focuses on the architecture of the system: components, interactions, and trade-offs.

## Core Concepts

### 1. Scalability
*   **Vertical Scaling (Scale Up)**: Adding more power (CPU, RAM) to a single server. Limited ceiling.
*   **Horizontal Scaling (Scale Out)**: Adding more servers. Infinite ceiling but requires distributed logic.

### 2. Load Balancing
Distributes traffic across servers to prevent overload.
*   **L4 (Transport Layer)**: Based on IP/Port. Fast.
*   **L7 (Application Layer)**: Based on URL/Cookie/Header. Smart but slower.
*   **Algorithms**: Round Robin, Least Connections, Consistent Hashing.

### 3. Caching
"There are only two hard things in Computer Science: cache invalidation and naming things."
*   **CDN (Content Delivery Network)**: Caches static assets (images, video) at the edge (close to user).
*   **Distributed Cache (Redis/Memcached)**: Caches DB queries or API responses in RAM.
*   **Strategies**:
    *   **Write-Through**: Write to DB and Cache simultaneously. Safe but slow write.
    *   **Write-Back**: Write to Cache, async write to DB. Fast write, risk of data loss.
    *   **Cache-Aside**: App checks cache. If miss, read DB and update cache.

## Advanced Topics

### 1. CAP Theorem
In a distributed system, you can only pick 2 out of 3:
*   **Consistency**: Every read receives the most recent write or an error.
*   **Availability**: Every request receives a (non-error) response, without the guarantee that it contains the most recent write.
*   **Partition Tolerance**: The system continues to operate despite an arbitrary number of messages being dropped or delayed by the network.
*   *Reality*: P is mandatory. You choose between CP (Banking) and AP (Social Media).

### 2. Database Scaling
*   **Sharding**: Horizontal partitioning. Split data by key (e.g., UserID) across multiple DB instances.
*   **Replication**: Master-Slave (Read replicas). Increases read throughput.
*   **Federation**: Splitting DBs by function (User DB, Product DB).

### 3. Message Queues (Kafka/RabbitMQ)
*   Decouples producers and consumers.
*   Handles backpressure (buffering bursts of traffic).
*   Ensures asynchronous processing.

## Interview Questions

### Senior Level
1.  **What is Consistent Hashing?**
    *   *Answer*: A hashing technique where adding/removing a slot (server) only affects $K/N$ keys. Used in Load Balancers and Distributed Caches to minimize reshuffling when a server dies.
2.  **Explain the difference between SQL and NoSQL.**
    *   *Answer*: SQL (ACID, Relational, Fixed Schema, Vertical Scale). NoSQL (BASE, Non-relational, Flexible Schema, Horizontal Scale). Use SQL for structured data/transactions. Use NoSQL for massive unstructured data/high throughput.
3.  **How do you prevent Cache Stampede?**
    *   *Answer*: When a popular key expires, thousands of requests hit the DB simultaneously. Fix: 1) Probabilistic early expiration. 2) Locking (only one process updates cache).

### Principal Level
1.  **Design a URL Shortener (TinyURL).**
    *   *Discussion*:
        *   **API**: `shorten(url)`, `redirect(short_url)`.
        *   **DB**: NoSQL (Key-Value). High read/write ratio.
        *   **hashing**: MD5/SHA256 (too long) vs Base62 encoding of a unique ID.
        *   **ID Generation**: Distributed ID Generator (Snowflake) or Zookeeper range allocation.
2.  **Design a Chat Application (WhatsApp).**
    *   *Discussion*:
        *   **Protocol**: WebSocket or MQTT for real-time bi-directional communication.
        *   **Storage**: Cassandra/HBase for chat history (write-heavy).
        *   **Status**: Heartbeat mechanism for Online/Offline.
        *   **Encryption**: End-to-End (Signal Protocol).
3.  **How to handle "Hot Keys" (Celebrity Problem) in a distributed system?**
    *   *Discussion*: If Justin Bieber tweets, millions read one key.
    *   *Fix*: Add random suffix to the key to spread load across shards. Replicate the hot key to multiple cache nodes.
