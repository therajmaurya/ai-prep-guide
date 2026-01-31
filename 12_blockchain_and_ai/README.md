---
title: "Blockchain & AI"
category: specialisation
levels: ["mid", "senior", "principal"]
skills: [blockchain, decentralization, decentralized-ai, smart-contracts, zkml, federated-learning, privacy, tokenomics]
questions:
  "mid": ["What is a Smart Contract?", "How does blockchain ensure data provenance?", "Why is running a Neural Network directly on Ethereum (Layer 1) impossible today?"]
  "senior": ["How can Blockchain improve AI data privacy?", "Explain the concept of ZKML.", "What is Federated Learning and how can Blockchain enhance it?"]
  "principal": ["Design a decentralized marketplace for AI models.", "Discuss the scalability challenges of on-chain AI.", "Discuss the feasibility of running model inference on-chain vs off-chain with ZK proofs setup."]
---

# Blockchain & AI

The intersection of Blockchain and AI (DeAI) is a rapidly evolving field that aims to democratize access to AI, ensure data privacy, and create transparent, verifiable AI systems. The convergence of these technologies offers solutions for decentralized compute, data privacy, and provenance.

## 💡 Core Concepts

### 1. Decentralized AI (DeAI) & Compute
- **Definition**: AI systems that run on decentralized networks rather than centralized servers (like AWS/GCP). This includes renting consumer GPUs for training/inference.
- **Benefits**: Censorship resistance, lower costs (potentially), and open access.
- **Key Projects**: Bittensor (decentralized ML network), Gensyn (decentralized compute), Render, Akash.

### 2. Data Provenance & Ownership
- **Problem**: In traditional AI, it's hard to track where data comes from and if it was used ethically.
- **Solution**: Blockchain provides an immutable ledger to record data sources, ensuring creators are compensated and data quality is verifiable.
- **Mechanism**: NFTs or on-chain registries for datasets.

### 3. Zero-Knowledge Machine Learning (ZKML) & Privacy
- **Concept**: Proving that a specific model was run on specific data without revealing the model weights or the input data.
- **Use Cases**:
    - **Privacy**: Running a medical diagnosis model on patient data without the model owner seeing the data.
    - **Verifiability**: Proving that a DeFi protocol used the correct risk model without revealing the proprietary model.
- **Technologies**: zk-SNARKs, zk-STARKs.

### 4. Federated Learning on Blockchain
- **Federated Learning (FL)**: Training a model across multiple devices without sharing raw data.
- **Blockchain's Role**: Orchestrates the FL process, incentivizes participants with tokens, and stores model updates immutably to prevent poisoning attacks.

### 5. AI Agents on Chain
- **Autonomous Agents**: AI agents that can own wallets, sign transactions, and interact with DeFi protocols autonomously.
- **Applications**: Automated trading bots, DAO management, cross-chain arbitrage.

## 📚 Resources
- **Whitepapers & Articles**:
    - [Vitalik Buterin on AI & Crypto](https://vitalik.eth.limo/general/2024/01/30/cryptoai.html) - Essential reading on the 4 intersections of AI and Crypto.
    - [Messari: The State of DePIN and AI](https://messari.io/) - Industry analysis.
    - [Gensyn Litepaper](https://docs.gensyn.ai/litepaper/) - Protocol for decentralized deep learning compute.
- **Projects & Organizations**:
    - [Decentralized AI Alliance](https://daalliance.org/) - Industry group.
    - [Ocean Protocol](https://oceanprotocol.com/) - Data sharing and monetization.
    - [Bittensor](https://bittensor.com/) - P2P intelligence market.
    - [Gensyn](https://gensyn.ai/) - Deep learning compute protocol.
    - [Modulus Labs](https://www.moduluslabs.xyz/) - ZKML solutions.

## 🛠️ Practice

### Project 1: Provenance Registry (Smart Contracts)
Write a Solidity Smart Contract that registers a model's hash and the hash of the dataset used to train it.
*   **Goal**: Create an immutable audit trail for model versions.

### Project 2: Paid Inference API
Build a simple specialized model API that accepts payments in a testnet cryptocurrency (e.g., Sepolia ETH) before returning the prediction.
*   **Goal**: Integrate Web3.py with a Flask/FastAPI model server.

### Project 3: DePIN Participation
Run a node for a decentralized compute network (if hardware allows) or simulate the interaction.

### Project 4: ZKML Proof Generation
Use a library like EZKL to generate a proof for a simple neural network inference.

## 🗣️ Interview Questions

### Mid Level
1.  **Why is running a Neural Network directly on Ethereum (Layer 1) impossible today?**
    *   *Answer*: Compute costs (Gas) are prohibitively high, and the EVM (Ethereum Virtual Machine) is not optimized for matrix operations. Block limits also restrict computation time.
2.  **What is a Smart Contract and how does it relate to AI?**
    *   *Answer*: Self-executing code on a blockchain. Can be used to automate payments for AI services or govern DAO-managed AI models.
3.  **How does blockchain ensure data provenance?**
    *   *Answer*: By recording the hash and metadata of data on an immutable ledger, creating a verifiable audit trail.
4.  **What is the difference between centralized and decentralized AI?**
    *   *Answer*: Centralized AI runs on controlled servers (single point of failure/control); DeAI runs on distributed networks (open, censorship-resistant).
5.  **What is a DAO?**
    *   *Answer*: Decentralized Autonomous Organization. Can be used to collectively own and manage AI models.

### Senior Level
1.  **What is Federated Learning, and how can Blockchain enhance it?**
    *   *Answer*: Federated Learning trains models across decentralized devices without sharing raw data. Blockchain can coordinate the global model aggregation, incentivize workers (tokens), and penalize malicious actors (slashing) in a trustless manner.
2.  **How can Blockchain improve AI data privacy?**
    *   *Answer*: Through techniques like Federated Learning (orchestrated on-chain) and ZKML, allowing computation without revealing raw data.
3.  **Explain the concept of ZKML (Zero-Knowledge Machine Learning).**
    *   *Answer*: Cryptographic method to prove a computation (inference/training) was done correctly without revealing inputs or model parameters.
4.  **What are the challenges of running AI on-chain?**
    *   *Answer*: High gas costs, storage limitations, and latency. Most "AI on-chain" actually involves off-chain compute with on-chain verification.
5.  **How can tokenomics incentivize RLHF (Reinforcement Learning from Human Feedback)?**
    *   *Answer*: Users earn tokens for providing high-quality feedback/rankings, which improves the model (e.g., similar to Hivemapper for mapping).

### Principal Level
1.  **Design a decentralized marketplace for AI models.**
    *   *Discussion*: Architecture involving IPFS/Arweave for storage, smart contracts for payments/access control, and a reputation system for model quality.
2.  **Discuss the feasibility of running model inference on-chain vs off-chain with ZK proofs setup.**
    *   *Discussion*: Discuss tradeoffs. On-chain is trustworthy but expensive/slow. Off-chain with ZK proofs allows verification of the computation (that the correct model ran on correct data) without re-running it on-chain, saving gas while maintaining trust.
3.  **Discuss the scalability challenges of integrating AI workloads with blockchain.**
    *   *Discussion*: Blockchains are slow databases. Solution involves Layer 2s, off-chain compute (DePIN), and optimistic or ZK verification bridges.
4.  **How would you prevent "model poisoning" in a decentralized training network?**
    *   *Discussion*: Staking mechanisms (slashing malicious actors), redundancy (multiple nodes run same task), and cryptographic verification of updates.
