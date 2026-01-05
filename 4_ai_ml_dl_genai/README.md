---
title: "AI / ML / Deep Learning / GenAI"
category: must
levels: ["mid", "senior", "principal"]
skills: [deep-learning, transformers, llms, diffusion, regularization]
questions:
  "mid": ["Explain the Vanishing Gradient problem and how ResNets solve it."]
  "senior": ["Contrast Self-Attention with Cross-Attention. Where is each used?"]
  "principal": ["How would you adapt a standard Transformer architecture for infinite context length?"]
---

# AI / ML / Deep Learning / GenAI

## 💡 Core Ideas

This is the heart of the guide. It covers the progression from classical ML to the latest Generative AI models.

*   **Classical ML**: Trees (XGBoost/LightGBM), SVMs, K-Means. Still vital for tabular data.
*   **Deep Learning Foundations**:
    *   **Architectures**: CNNs (Vision), RNNs/LSTMs (Sequential), Transformers (Everything).
    *   **Training**: Backpropagation, Loss Functions (Cross-Entropy, MSE, Contrastive), Regularization (Dropout, BatchNorm, LayerNorm).
*   **Transformers & LLMs**:
    *   **Attention Mechanisms**: Multi-Head Attention, FlashAttention, Sliding Window Attention.
    *   **LLM Training**: Pre-training (Next Token Prediction), Fine-tuning (SFT), RLHF/DPO.
    *   **Efficient Tuning**: LoRA, QLoRA, Adapters.
*   **Generative AI**:
    *   **Diffusion Models**: Forward/Reverse process, UNet backbone, Stable Diffusion.
    *   **VAEs**: Encoder-Decoder, Latent space, Reparameterization trick.

## 📚 Resources

*   [**Deep Learning Book**](https://www.deeplearningbook.org/) - The bible by Goodfellow et al.
*   [**The Annotated Transformer**](https://nlp.seas.harvard.edu/2018/04/03/attention.html) - Line-by-line code walk-through.
*   [**Stanford CS231n (Vision) & CS224n (NLP)**](http://cs231n.stanford.edu/) - The gold standard academic courses.
*   [**Hands-On Machine Learning**](https://www.oreilly.com/library/view/hands-on-machine-learning/9781492032632/) - Great for classical ML foundations.
*   [**Fast.ai**](https://www.fast.ai/) - Practical Deep Learning for Coders.
*   [**Lilian Weng's Blog**](https://lilianweng.github.io/) - In-depth technical posts on LLMs and Agents.

## 🛠️ Practice

### Project 0: The Classics (NumPy Only)
Build a regularized Logistic Regression and a K-Nearest Neighbors classifier from scratch using **only NumPy**.
*   **Goal**: Implement the forward pass, loss derivation, and backward pass (gradient update) manually.
*   **Why**: If you can't derive the gradient for LogReg, you won't understand how Llama-3 is trained.

### Project 1: Transformer from Scratch
Implement a mini-GPT model in PyTorch following the "Attention is All You Need" paper structure.
*   **Goal**: Train it on Shakespeare text to generate Shakespeare-like sentences.

### Project 2: LoRA Fine-Tuning
Fine-tune a small LLM (like Llama-3-8B or Mistral 7B) on a specific dataset (e.g., medical Q&A) using LoRA (Low-Rank Adaptation).
*   **Goal**: Compare the performance and memory usage vs full fine-tuning.

### Project 3: Diffusion Model
Build a simple Denoising Diffusion Probabilistic Model (DDPM) to generate MNIST digits.

## 🗣️ Interview Questions

### Mid-Level
*   **Q**: What is the difference between Batch Normalization and Layer Normalization? Why do Transformers use LayerNorm?
*   **A**: BatchNorm normalizes across the batch dimension (dependent on batch size), while LayerNorm normalizes across the feature dimension (independent of batch size). Transformers use LayerNorm because it works better with variable sequence lengths and small batches.

### Senior-Level
*   **Q**: Explain RLHF (Reinforcement Learning from Human Feedback). What are the main steps?
*   **A**:
    1.  **SFT**: Supervised Fine-Tuning on high-quality instruction data.
    2.  **Reward Model**: Train a model to predict human preference (A > B).
    3.  **PPO/DPO**: Optimize the Policy (LLM) to maximize the reward while staying close to the original model (KL penalty).

### Principal-Level
*   **Q**: Discuss the trade-offs between Mixture of Experts (MoE) and Dense models of the same parameter count.
*   **A**:
    *   **MoE**: Higher total parameters (capacity) but lower active parameters per token (inference speed). Harder to train/stabilize, potential for load balancing issues (expert collapse), high VRAM requirement for weights.
    *   **Dense**: Easier to train, better hardware utilization (dense matmuls), but slower inference for same capacity.
    *   **Trade-off**: MoE is better for scaling throughput at the cost of VRAM and system complexity.
