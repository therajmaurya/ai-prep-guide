---
title: "Recommendation Systems"
category: must
levels: ["senior", "principal"]
skills: [relevancy, ranking, two-tower, dlrm, collaborative-filtering]
questions:
  "mid": ["Explain Collaborative Filtering vs Content-Based Filtering."]
  "senior": ["Discuss the 'Cold Start' problem and 3 distinct ways to solve it."]
  "principal": ["Design the ranking layer for a news feed with 10M DAU. How do you handle diversity?"]
---

# Recommendation Systems

## 💡 Core Ideas

RecSys is the biggest revenue driver for big tech. It's distinct from standard classification because of the **scale** (millions of users/items) and **sparsity**.

*   **Architecture (The Funnel)**:
    1.  **Candidate Generation (Retrieval)**: Select top 1000 items from 100M. Fast, high recall. (Matrix Factorization, Two-Tower ANN).
    2.  **Scoring (Ranking)**: Score the 1000 items precisely. Slower, high precision. (DLRM, XGBoost, Cross-Encoders).
    3.  **Re-Ranking**: Apply business logic (diversity, freshness, ads insertion).
*   **Techniques**:
    *   **Matrix Factorization**: Decompose User-Item Interaction matrix ($R \approx U \times V^T$).
    *   **Embeddings**: Learning vector representations for userIDs and itemIDs.
    *   **Wide & Deep**: Combines memorization (Wide linear model for cross-products) and generalization (Deep MLP).
    *   **Two-Tower**: User Encoder and Item Encoder output 2 vectors. Score = Dot Product (fast ANN search).
*   **Feedback**:
    *   **Explicit**: Ratings (1-5 stars), Likes.
    *   **Implicit**: Clicks, Dwell time, Purchases, Scroll depth. Implicit data is vastly more abundant.

*   **Content-based vs Collaborative Filtering**.
*   **Matrix Factorization (ALS, SVD)**.
*   **Deep Learning for RecSys (Two-Tower, DLRM)**.
*   **Evaluation Metrics (NDCG, MAP)**.

## 📚 Resources

*   [**Google Recommendation Systems Course**](https://developers.google.com/machine-learning/recommendation) - Excellent and free.
*   [**System Design for Recommendations**](https://eugeneyan.com/writing/system-design-for-discovery/) - Eugene Yan's blog.
*   [**Deep Learning Recommendation Model (DLRM)**](https://arxiv.org/abs/1906.00091) - Facebook's open source state-of-the-art model.
*   [System Design for Recommendations and Search](https://www.amazon.com/System-Design-Recommendations-Search-e-commerce/dp/1098127791)

## 🛠️ Practice

### Project 1: Movie Recommender (Two-Tower)
Build a two-tower model in Keras/PyTorch for the MovieLens dataset.
*   **Goal**: Train User Tower and Movie Tower. Export embeddings to Faiss. Perform fast retrieval to find movies for a user.

### Project 2: Session-Based Recs
Use a Transformer (like BERT4Rec) to predict the *next* item a user will view based on their session history [item1, item2, item3...].
*   **Goal**: Compare against a simple Markov Chain baseline.

### Project 3: Movie Recommender (Matrix Factorization)
Build a movie recommender using Matrix Factorization.

### Project 4: Session-Based Recs (RNN)
Implement a session-based recommender using RNNs.

## 🗣️ Interview Questions

### Mid-Level
*   **Q**: What is the cold-start problem?
*   **A**: When a new user or item enters the system, there is no historical data to make recommendations. Solutions include content-based filtering, collaborative filtering with default profiles, or hybrid approaches.
*   **Q**: What is the difference between Precision@K and Recall@K?
*   **A**:
    *   **Precision@K**: Of the top K items recommended, how many were relevant? (User experience).
    *   **Recall@K**: Of all relevant items in the universe, how many were found in the top K? (System coverage).

### Senior-Level
*   **Q**: What is the "Position Bias" in ranking data and how do you handle it?
*   **A**: Users are more likely to click the top result just because it's first, not because it's better.
    *   **Fix**: During training, use the position as a feature (e.g., in a separate tower) to explain the click, but set the position feature to a constant (e.g., 0 or "neutral") during inference (Unbiasing).
*   **Q**: Discuss the trade-offs between diversity and relevance.
*   **A**: Diversity ensures variety, but relevance ensures quality. Solutions include using a separate tower for diversity (e.g., a tower that outputs a vector of 1s and 0s, where 1 means the item is diverse and 0 means it is not) and using a separate tower for relevance (e.g., a tower that outputs a vector of 1s and 0s, where 1 means the item is relevant and 0 means it is not).

### Principal-Level
*   **Q**: Explain "Exploration" in RecSys (Bandits). Why is a greedy algorithm bad?
*   **A**: A greedy algorithm only shows what the user liked in the past, leading to a "Filter Bubble" and eventual boredom (churn).
    *   **Contextual Multi-Armed Bandits (Thompson Sampling / UCB)**: Deliberately serve high-uncertainty items to learn user preferences. The cost (showing a bad item) is an investment in future engagement.
