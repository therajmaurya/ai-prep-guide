---
title: "Natural Language Processing"
category: nlp
levels: ["mid", "senior", "principal"]
skills: [nlp, transformers, tokenization, embeddings]
questions:
  "mid": ["What is Tokenization?", "Explain Word2Vec (CBOW vs Skip-gram).", "What is TF-IDF?"]
  "senior": ["Explain the difference between BERT and GPT.", "How does BPE (Byte Pair Encoding) work?", "What is Positional Encoding?"]
  "principal": ["Discuss the scaling laws of LLMs.", "How to optimize inference for Transformers (KV Cache)?", "Explain Rotary Positional Embeddings (RoPE)."]
---

# Natural Language Processing (NLP)

NLP has evolved from rule-based systems to statistical methods (HMMs) to RNNs and now Transformers (LLMs).

## Core Concepts

### 1. Text Preprocessing
*   **Tokenization**: Splitting text into units (words, subwords, characters).
*   **Stemming/Lemmatization**: Reducing words to root form (running -> run).
*   **Stop Words**: Removing common words (the, is, at). *Note: Modern LLMs often keep them.*

### 2. Embeddings
Representing words as dense vectors where similar words are close in space.
*   **Word2Vec**:
    *   **CBOW**: Predict target word from context.
    *   **Skip-gram**: Predict context words from target.
*   **GloVe**: Matrix factorization of co-occurrence counts.
*   **Contextual Embeddings**: ELMo, BERT. The vector for "bank" depends on whether it's a river bank or financial bank.

### 3. RNNs & LSTMs
*   **RNN**: Processes sequence step-by-step. Hidden state $h_t = f(W x_t + U h_{t-1})$. Issues: Vanishing gradient.
*   **LSTM/GRU**: Gating mechanisms (Input, Forget, Output gates) to control information flow and preserve long-term dependencies.

## Advanced Topics

### 1. Transformers
*   **Encoder-Only (BERT)**: Bidirectional. Good for understanding (Classification, NER). Masked Language Modeling (MLM).
*   **Decoder-Only (GPT)**: Unidirectional (Causal). Good for generation. Next Token Prediction.
*   **Encoder-Decoder (T5, Bart)**: Good for translation, summarization.

### 2. Subword Tokenization
*   **BPE (Byte Pair Encoding)**: Iteratively merges the most frequent pair of characters/tokens. Solves OOV (Out of Vocabulary) problem.
*   **WordPiece**: Similar to BPE but maximizes likelihood of language model. Used in BERT.
*   **SentencePiece**: Language-independent, treats input as raw stream of unicode characters (includes spaces).

### 3. Positional Encoding
*   Transformers process tokens in parallel, so they have no notion of order.
*   **Sinusoidal**: Fixed sine/cosine functions added to embeddings.
*   **Learned**: Learnable vectors.
*   **RoPE (Rotary Positional Embedding)**: Rotates the query/key vectors. Relative position is preserved via rotation. Standard in LLaMA.

## Interview Questions

### Mid Level
1.  **What is TF-IDF?**
    *   *Answer*: Term Frequency - Inverse Document Frequency. Measures importance of a word in a document relative to a corpus. High if word is frequent in doc but rare in corpus.
2.  **Why do we use Subword Tokenization?**
    *   *Answer*: To handle rare words and OOV issues without a massive vocabulary. "Unfriendly" -> "Un", "friend", "ly".
3.  **What is the difference between Stemming and Lemmatization?**
    *   *Answer*: Stemming chops off ends (heuristic). Lemmatization uses a dictionary/morphological analysis to find the actual root (lemma).

### Senior Level
1.  **Explain the difference between BERT and GPT.**
    *   *Answer*: BERT is an Encoder (Bidirectional). It sees the whole sentence. Trained on Masked LM. Good for classification. GPT is a Decoder (Autoregressive). It only sees past tokens. Trained on Next Token Prediction. Good for generation.
2.  **How does the Self-Attention mechanism work?**
    *   *Answer*: For each token, it computes a weighted sum of all other tokens. Weights are determined by dot product of Query and Key vectors. $Attention(Q, K, V) = softmax(QK^T / \sqrt{d})V$.
3.  **What is Beam Search?**
    *   *Answer*: A heuristic search algorithm for generation. Instead of picking the single best next token (Greedy), it keeps track of the top $k$ (beam width) most probable sequences at each step.

### Principal Level
1.  **Explain KV Cache in Transformer Inference.**
    *   *Discussion*: In autoregressive generation, previous tokens' Keys and Values don't change. We cache them to avoid recomputing attention for the entire history at every step. Reduces complexity from $O(N^2)$ to $O(N)$ per token.
2.  **Discuss the Scaling Laws (Chinchilla).**
    *   *Discussion*: Performance scales as a power law with parameters ($N$) and data ($D$). Chinchilla found that for a given compute budget, model size and data size should be scaled equally. Most models were undertrained.
3.  **How does RoPE (Rotary Positional Embedding) work?**
    *   *Discussion*: It encodes relative position by rotating the affine-transformed word embedding vector by an amount of angle multiples of its position index. It allows the attention score $q^T k$ to depend only on relative distance $(m-n)$.
