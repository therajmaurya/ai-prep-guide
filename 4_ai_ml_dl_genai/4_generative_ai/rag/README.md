---
title: "Retrieval Augmented Generation (RAG)"
category: agentic-ai
levels: ["mid", "senior", "principal"]
skills: [rag, vector-databases, embeddings, llms]
questions:
  "mid": ["What is RAG?", "Explain Semantic Search.", "What is a Vector Database?"]
  "senior": ["Explain HNSW indexing.", "What is Hybrid Search?", "How to handle 'Lost in the Middle' problem?"]
  "principal": ["Design an Advanced RAG pipeline (HyDE, Re-ranking).", "How to optimize RAG latency?", "Explain GraphRAG."]
---

# Retrieval Augmented Generation (RAG)

RAG combines the reasoning power of LLMs with external, up-to-date knowledge. It solves hallucinations and knowledge cutoff issues.

## Core Concepts

### 1. The Pipeline
1.  **Ingestion**: Load documents (PDF, HTML).
2.  **Chunking**: Split text into smaller pieces (Fixed size, Recursive, Semantic).
3.  **Embedding**: Convert chunks into vectors using an Embedding Model (OpenAI, Cohere, HuggingFace).
4.  **Storage**: Save vectors in a Vector DB (Pinecone, Milvus, Chroma).
5.  **Retrieval**: Convert user query to vector. Find nearest neighbors (Cosine Similarity).
6.  **Generation**: Pass retrieved chunks + query to LLM.

### 2. Vector Databases
*   Specialized DBs optimized for high-dimensional vector search.
*   **Indexing**: Brute force (KNN) is too slow ($O(N)$). We use ANN (Approximate Nearest Neighbors).
*   **HNSW (Hierarchical Navigable Small World)**: Graph-based index. Fast and accurate. Standard in most Vector DBs.
*   **IVF (Inverted File Index)**: Clusters vectors. Search only nearest clusters.

### 3. Semantic Search
*   Searching by meaning, not keywords.
*   "King - Man + Woman = Queen".
*   Handles synonyms and intent.

## Advanced Topics

### 1. Advanced RAG Techniques
*   **Hybrid Search**: Combine Vector Search (Semantic) with Keyword Search (BM25). Best of both worlds (exact matches + conceptual matches).
*   **Re-ranking**: Retrieve top 50 chunks (fast), then use a Cross-Encoder (slow, accurate) to re-rank them and pick top 5 for the LLM.
*   **HyDE (Hypothetical Document Embeddings)**: LLM generates a fake answer to the query. Embed the fake answer and search for real documents similar to it.

### 2. Chunking Strategies
*   **Fixed Size**: Simple but breaks context.
*   **Recursive**: Split by paragraphs, then sentences. Preserves structure.
*   **Semantic**: Split based on meaning changes (using embeddings).
*   **Parent-Child**: Retrieve small chunks (Child) for search accuracy, but feed the larger surrounding context (Parent) to the LLM.

### 3. GraphRAG
*   Constructs a Knowledge Graph from documents.
*   Retrieval traverses the graph to find relationships.
*   Better for "global" questions ("What are the main themes in this dataset?") where vector search fails.

## Interview Questions

### Mid Level
1.  **Why do we need Chunking?**
    *   *Answer*: 1) LLM Context Window limits. 2) Retrieval granularity: We want to find the specific paragraph that answers the question, not the whole book.
2.  **What is Cosine Similarity?**
    *   *Answer*: Measure of similarity between two vectors. Cosine of the angle between them. $A \cdot B / (||A|| ||B||)$. 1 = Identical, 0 = Orthogonal, -1 = Opposite.
3.  **What is the difference between Fine-Tuning and RAG?**
    *   *Answer*: Fine-Tuning teaches the model *how* to behave (style, format) or specific domain language. RAG gives the model *what* to know (facts, data). RAG is cheaper and easier to update.

### Senior Level
1.  **Explain HNSW (Hierarchical Navigable Small World).**
    *   *Answer*: It's a multi-layer graph. Top layers have long links (highways) for fast traversal across the space. Bottom layers have short links for fine-grained search. Like zooming in on a map.
2.  **What is the "Lost in the Middle" phenomenon?**
    *   *Answer*: LLMs tend to pay more attention to information at the beginning and end of the context window, ignoring the middle.
    *   *Fix*: Re-order retrieved chunks so the most relevant ones are at the start/end.
3.  **How does Hybrid Search (Reciprocal Rank Fusion) work?**
    *   *Answer*: Run Vector Search and Keyword Search (BM25) in parallel. Assign a score to each result based on its rank in both lists ($1 / (k + rank)$). Combine scores to get final ranking.

### Principal Level
1.  **Design a RAG system for a codebase (Copilot).**
    *   *Discussion*:
        *   **Chunking**: By function/class. Keep imports and signatures.
        *   **Graph**: Build a dependency graph (Call Hierarchy).
        *   **Context**: Include file path, related files.
        *   **Privacy**: Don't send PII to public LLM APIs.
2.  **How to optimize RAG latency?**
    *   *Discussion*:
        *   **Parallelization**: Run retrieval and other tasks async.
        *   **Caching**: Cache embeddings of frequent queries.
        *   **Quantization**: Use int8 embeddings.
        *   **Speculative RAG**: Start generating answer while retrieving (if confident).
3.  **Explain the limitations of Vector Search.**
    *   *Discussion*: It loses exact keyword matching (can't distinguish "Version 1.0" vs "Version 2.0" easily). It struggles with structured queries ("Find documents from 2023"). It lacks reasoning capabilities (GraphRAG helps).
