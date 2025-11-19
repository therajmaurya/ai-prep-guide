---
title: "Distributed System Design"
category: specialisation
levels: ["senior", "principal"]
skills: [distributed-systems, consensus, replication]
questions:
  "mid": ["What is consistent hashing?"]
  "senior": ["Design a distributed key-value store like Dynamo."]
---

# Distributed System Design

## Core Ideas
- Consensus Algorithms (Paxos, Raft).
- Replication (Master-Slave, Multi-Master).
- Partitioning/Sharding.
- Eventual Consistency vs Strong Consistency.

## Resources
- [Distributed Systems for Fun and Profit](http://book.mixu.net/distsys/)
- [Jepsen Analyses](https://jepsen.io/analyses)

## Practice
- Implement a simple leader election algorithm.
- Simulate a network partition and observe system behavior.

## Questions
### Mid Level
- Explain the Two-Phase Commit protocol.

### Senior Level
- How to handle clock skew in distributed systems?
