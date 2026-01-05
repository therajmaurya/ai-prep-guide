---
title: "Chunking Strategies for RAG"
category: specialisation
levels: ["senior", "principal"]
skills: [rag, chunking, embeddings, parsing]
questions:
  "mid": ["Why can't we just feed the entire PDF into the context window?"]
  "senior": ["Compare Fixed-size chunking vs Semantic chunking."]
  "principal": ["Design a parsing pipeline for complex financial PDFs with tables and multi-column layouts."]
---

# Chunking Strategies for RAG

## 💡 Core Ideas

"Garbage In, Garbage Out". If you slice a sentence in half, the embedding will be meaningless. Chunking is the most critical step in RAG.

*   **Fixed-Size Chunking**:
    *   Split by characters/tokens (e.g., 500 tokens) with overlap (e.g., 50 tokens). Fast but blind to context.
*   **Recursive Character Chunking**:
    *   Split by separators (`\n\n`, `\n`, `.`, ` `) recursively. Keeps paragraphs together.
*   **Semantic Chunking**:
    *   Embed every sentence.
    *   Calculate cosine similarity between sequential sentences.
    *   Break the chunk only when similarity drops below a threshold (topic change).
*   **Agentic / Propositional Chunking**:
    *   Use an LLM to read the text and extract standalone propositions/facts. Expensive but highest quality.
*   **Late Chunking**:
    *   TBD

## Chunking Evaluation
* TBD

## 📚 Resources

*   [**Five Levels of Text Splitting**](https://github.com/FullStackRetrieval-com/RetrievalTutorials/blob/main/5_levels_of_text_splitting.ipynb) - Excellent notebook tutorial.
*   [**Unstructured.io**](https://unstructured.io/) - Tooling for parsing PDFs/HTML.

## 🛠️ Practice

### Project 1: Semantic Chunker
Implement a semantic chunker using `sentence-transformers`.
*   **Goal**: Visualize the similarity scores between sentences as a heatmap and see where the "breaks" occur.

### Project 2: Parent Document Retriever
Implement a "Small-to-Big" strategy.
*   **Goal**: Index small chunks (sentences) for search, but retrieve the *parent* chunk (paragraph/page) for the LLM context.

## 🗣️ Interview Questions

### Senior-Level
*   **Q**: How do you handle Tables in PDFs for RAG?
*   **A**: Standard chunking destroys tables.
    1.  **Table Extraction**: Use libraries like `Tabula` or `Unstructured` (or Vision Models like GPT-4o) to detect and extract tables as markdown/HTML.
    2.  **Summary Embedding**: Ask an LLM to summarize the table. Embed the *summary*.
    3.  **Raw Retrieval**: If retrieved, return the full HTML/Markdown table to the context.

### Principal-Level
*   **Q**: When would you choose "GraphRAG" (Knowledge Graphs) over Vector Search?
*   **A**:
    *   **Vector Search**: Good for "similarity" (Find documents about X). Bad for "reasoning" or "relationships" (How is A related to B via C?).
    *   **GraphRAG**: Good for multi-hop reasoning. If the query requires traversing entities (e.g., "Find all subsidiaries of companies that competitor X acquired"), a Knowledge Graph is superior.
