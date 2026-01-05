---
title: "Distributed System Design"
category: system-design
levels: ["senior", "principal"]
skills: [distributed-systems, consensus, mapreduce, consistency]
questions:
  "senior": ["Explain MapReduce.", "What is Consistent Hashing?", "How does Gossip Protocol work?"]
  "principal": ["Explain Paxos or Raft consensus.", "Design a Distributed Key-Value Store (DynamoDB).", "How to handle Distributed Transactions (2PC vs Saga)?"]
---

# Distributed System Design

Distributed Systems involve multiple computers working together as a single system. The challenge is handling failure and maintaining consistency.

## Core Concepts

### 1. Consensus Algorithms
How do nodes agree on a value (e.g., who is the leader)?
*   **Paxos**: The original, mathematically proven, but very hard to understand/implement.
*   **Raft**: Designed to be understandable. Uses Leader Election and Log Replication. Used in Etcd, Consul.

### 2. Data Partitioning
*   **Sharding**: Splitting data across nodes.
*   **Replication**: Copying data for availability.
*   **Consistent Hashing**: Maps keys to a "ring" of servers. Minimizes data movement when scaling.

### 3. Distributed Computing Models
*   **MapReduce**:
    *   **Map**: Process data in parallel chunks.
    *   **Shuffle**: Group data by key.
    *   **Reduce**: Aggregate results.
*   **Spark**: In-memory processing. DAG (Directed Acyclic Graph) execution. Faster than MapReduce.

## Advanced Topics

### 1. Distributed Transactions
*   **ACID** is hard across nodes.
*   **2PC (Two-Phase Commit)**:
    1.  **Prepare**: Coordinator asks all nodes "Can you commit?"
    2.  **Commit**: If all say yes, Coordinator says "Commit". If one says no, "Abort".
    *   *Problem*: Blocking. If Coordinator dies, system hangs.
*   **Saga Pattern**: Sequence of local transactions. If one fails, execute compensating transactions to undo previous steps. Better for microservices.

### 2. Consistency Models
*   **Strong Consistency**: Everyone sees the same data at the same time (Paxos/Raft). Slow.
*   **Eventual Consistency**: Everyone will eventually see the same data (Gossip). Fast.
*   **Causal Consistency**: If A causes B, everyone sees A before B.
*   **CAP Theorem**: Pick two of Consistency, Availability, Partition Tolerance. (In reality, P is mandatory, so pick CP or AP).

### 3. Scaling Vector Databases (New for AI)
*   **IVF-PQ (Inverted File with Product Quantization)**: Approximate Nearest Neighbor.
*   **Sharding**:
    *   **By ID**: Good for point lookups.
    *   **By Clustering**: Group similar vectors on the same node (reduces search scope).
*   **Replication**: For high QPS read throughput.

### 3. Failure Detection
*   **Heartbeats**: Nodes send "I'm alive" signals.
*   **Gossip Protocol**: Nodes randomly tell neighbors about state. Information spreads like a virus. Used in Cassandra/Dynamo.

## Interview Questions

### Senior Level
1.  **Explain the MapReduce word count example.**
    *   *Answer*:
        *   **Map**: Input "Hello World" -> ("Hello", 1), ("World", 1).
        *   **Shuffle**: Group all ("Hello", [1, 1, ...]).
        *   **Reduce**: Sum counts -> ("Hello", 2).
2.  **What is a Bloom Filter?**
    *   *Answer*: A probabilistic data structure to test if an element is in a set.
    *   *Properties*: False Positives are possible. False Negatives are impossible. Very space efficient. Used to avoid disk lookups for non-existent keys.
3.  **How does Cassandra handle writes?**
    *   *Answer*: 1) Write to Commit Log (disk) for durability. 2) Write to MemTable (RAM). 3) When MemTable is full, flush to SSTable (disk).

### Principal Level
1.  **Explain Raft Leader Election.**
    *   *Discussion*: Nodes are in one of 3 states: Follower, Candidate, Leader. If Follower doesn't hear from Leader (timeout), it becomes Candidate, increments term, votes for itself, and asks for votes. If it gets majority, it becomes Leader.
2.  **Design a Distributed Lock Manager.**
    *   *Discussion*:
        *   **Redis (Redlock)**: Acquire lock on N instances. If successful on N/2 + 1, lock acquired.
        *   **Zookeeper/Etcd**: Use ephemeral nodes. If node exists, lock is taken. If client dies, node disappears (lock released).
3.  **Compare 2PC and Saga.**
    *   *Discussion*: 2PC provides atomicity but locks resources and has single point of failure. Saga provides eventual consistency (ACID -> BASE) and is more scalable but requires writing complex compensation logic.
