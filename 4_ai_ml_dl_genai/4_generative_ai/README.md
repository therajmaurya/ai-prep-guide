---
title: "Generative AI"
category: genai
levels: ["mid", "senior", "principal"]
skills: [generative-ai, llms, diffusion-models, gans]
questions:
  "mid": ["What is a VAE?", "Explain the concept of GANs.", "What is Prompt Engineering?"]
  "senior": ["Explain the Diffusion Process (Forward vs Reverse).", "How does RLHF work?", "What is the Reparameterization Trick?"]
  "principal": ["Derive the ELBO for VAEs.", "Discuss PPO vs DPO for LLM alignment.", "Explain the architecture of Stable Diffusion (Latent Diffusion)."]
---

# Generative AI

Generative AI focuses on creating new data (images, text, audio) that resembles the training distribution.

## Core Concepts

### 1. VAEs (Variational Autoencoders)
*   **Goal**: Learn a latent space $z$ such that we can sample $z \sim N(0, I)$ and decode it to generate data $x$.
*   **Encoder**: $q(z|x)$. Maps input to mean $\mu$ and variance $\sigma$.
*   **Decoder**: $p(x|z)$. Reconstructs input from sampled $z$.
*   **Loss**: Reconstruction Loss + KL Divergence (regularizes latent space to be Gaussian).

### 2. GANs (Generative Adversarial Networks)
*   **Generator ($G$)**: Tries to fool the discriminator by creating fake data.
*   **Discriminator ($D$)**: Tries to distinguish real data from fake data.
*   **Minimax Game**: $\min_G \max_D V(D, G)$.
*   **Issues**: Mode Collapse (generating same output), Training Instability.

### 3. Diffusion Models
*   **Forward Process**: Gradually add Gaussian noise to data until it becomes pure noise.
*   **Reverse Process**: Train a neural network (U-Net) to predict the noise added at each step and remove it.
*   **State of the Art**: DALL-E 3, Stable Diffusion, Midjourney.

## Advanced Topics

### 1. LLMs & Alignment
*   **Pre-training**: Next token prediction on massive corpus. Learns world knowledge.
*   **SFT (Supervised Fine-Tuning)**: Fine-tune on high-quality instruction-response pairs. Learns to follow instructions.
*   **RLHF (Reinforcement Learning from Human Feedback)**:
    1.  Train Reward Model (RM) on human rankings.
    2.  Optimize Policy (LLM) using PPO to maximize reward while staying close to original model (KL penalty).
*   **DPO (Direct Preference Optimization)**: Optimizes policy directly on preferences without an explicit Reward Model. More stable than PPO.

### 2. Latent Diffusion
*   Running diffusion in pixel space is expensive.
*   **Stable Diffusion**: Compresses image into a lower-dimensional "latent space" using a VAE. Runs diffusion in latent space. Decodes back to pixels.

### 3. Retrieval Augmented Generation (RAG)
*   Combines parametric memory (LLM weights) with non-parametric memory (Vector DB).
*   Retrieves relevant chunks based on query embedding and injects them into the context window. Reduces hallucinations.

## Interview Questions

### Mid Level
1.  **What is Mode Collapse in GANs?**
    *   *Answer*: When the generator finds one output that fools the discriminator and keeps generating only that (e.g., only generating the digit "1" instead of 0-9).
2.  **What is Temperature in LLM sampling?**
    *   *Answer*: A hyperparameter that scales the logits before Softmax. High temp (>1) flattens distribution (more random/creative). Low temp (<1) sharpens it (more deterministic).
3.  **Explain the difference between Autoencoder and VAE.**
    *   *Answer*: AE learns a deterministic mapping (point to point). VAE learns a probabilistic mapping (point to distribution). VAE allows sampling new data; AE is mainly for compression/denoising.

### Senior Level
1.  **Explain the ELBO (Evidence Lower Bound).**
    *   *Answer*: In VAEs, we want to maximize $P(x)$. This is intractable. Instead, we maximize a lower bound: $E_{q(z|x)}[\log p(x|z)] - KL(q(z|x) || p(z))$.
2.  **How does Stable Diffusion condition on text?**
    *   *Answer*: Uses Cross-Attention layers in the U-Net. Text is encoded by CLIP/T5, and these embeddings act as Keys/Values for the image features (Queries).
3.  **What is the difference between PPO and DPO?**
    *   *Answer*: PPO requires training a separate Reward Model and uses RL (complex, unstable). DPO derives a loss function directly from the preference data, implicitly optimizing the same objective as RLHF but as a simple classification problem.

### Principal Level
1.  **Derive the Diffusion Loss function.**
    *   *Discussion*: It simplifies to a weighted Mean Squared Error between the true noise $\epsilon$ and the predicted noise $\epsilon_\theta(x_t, t)$.
2.  **Discuss the limitations of RAG.**
    *   *Discussion*: Retrieval accuracy (garbage in, garbage out), Context window limits (though growing), "Lost in the Middle" phenomenon (LLMs focus on start/end of context), Latency.
3.  **How would you detect hallucinations in LLMs?**
    *   *Discussion*: Self-consistency (sample multiple times, check agreement), Fact-checking via external tools, Log-probability checks (low prob tokens might be hallucinations), NLI (Natural Language Inference) models to check entailment against source.
