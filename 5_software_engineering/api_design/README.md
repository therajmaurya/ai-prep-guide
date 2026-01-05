---
title: "API Design for ML"
category: software-engineering
levels: ["senior", "principal"]
skills: [rest, grpc, graphql, pydantic, protobuf]
questions:
  "mid": ["REST vs gRPC: Key differences?"]
  "senior": ["Design an API schema for an Image Segmentation service."]
  "principal": ["How do you handle schema evolution (backward compatibility) in gRPC?"]
---

# API Design for ML

## 💡 Core Ideas

How do users/systems talk to your model?

*   **REST (Representational State Transfer)**:
    *   Standard, uses JSON over HTTP/1.1.
    *   Easy to debug (cURL).
    *   **Overhead**: JSON serialization/deserialization is slow for large floating point arrays.
    *   **Tools**: FastAPI, Flask.
*   **gRPC (Google Remote Procedure Call)**:
    *   Uses **Protobuf** (Binary format) over HTTP/2.
    *   Strictly typed (Schema-first).
    *   High performance, bi-directional streaming.
*   **GraphQL**: Flexible, query-based API.
*   **Schema Validation**: Pydantic (Python) ensures inputs match expected types before hitting the model.
* Authentication: JWT, OAuth.
* Authorization: Role-based access control.
* Rate Limiting: Redis, Leaky Bucket.

## Resources
- [API Design Patterns](https://manning.com/books/api-design-patterns)
- [gRPC in Action](https://manning.com/books/grpc-in-action)

## 🛠️ Practice

### Project 1: Dual Protocol Service
Build a service that exposes **both** a REST endpoint `/predict` and a gRPC method `Predict()`.
*   **Goal**: Benchmark the latency difference when sending a 4MB image payload.

### Project 2: Design a REST API for a blog.

### Project 3: Implement a GraphQL server using Apollo or Graphene.

## 🗣️ Interview Questions

### Mid-Level
*   **Q**: REST vs gRPC: Key differences?
*   **A**:
    1.  **REST**: Stateless, uses JSON over HTTP/1.1.
    2.  **gRPC**: Stateful, uses Protobuf over HTTP/2.
*   **Q**: What are the safe HTTP methods?
*   **A**: GET, HEAD, OPTIONS, TRACE.
*   **Q**: What are the idempotent HTTP methods?
*   **A**: GET, HEAD, PUT, DELETE, OPTIONS, TRACE.

### Senior-Level
*   **Q**: When should you use gRPC over REST?
*   **A**:
    1.  **Internal Microservices**: Low latency communication.
    2.  **Streaming**: Real-time voice/video processing.
    3.  **Large Payloads**: Binary Protobuf is smaller/faster than base64 encoded JSON.

### Principal-Level
*   **Q**: How do you handle schema evolution (backward compatibility) in gRPC?
*   **A**: Use **Semantic Versioning** (major, minor, patch).
*   **Q**: Discuss the pros and cons of HATEOAS.
*   **A**: HATEOAS (Hypermedia as the Engine of Application State) allows clients to discover and navigate resources dynamically.
