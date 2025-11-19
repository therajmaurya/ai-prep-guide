---
title: "SWE/SDE System Design"
category: must
levels: ["mid", "senior"]
skills: [web-systems, databases, caching]
questions:
  "mid": ["Design a key-value store."]
  "senior": ["Design a distributed job scheduler."]
---

# SWE/SDE System Design

## Core Ideas
- Standard web architecture (Client -> LB -> App Server -> DB).
- Asynchronous processing (Message Queues).
- Database choice (SQL vs NoSQL).

## Resources
- [Grokking the System Design Interview](https://www.educative.io/courses/grokking-the-system-design-interview)

## Practice
- Design Instagram News Feed.
- Design Uber backend.

## Questions
### Mid Level
- How to handle hot partitions in a database?

### Senior Level
- Design a system to handle 1M concurrent websocket connections.
