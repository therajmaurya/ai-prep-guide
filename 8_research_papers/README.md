---
title: "Seminal Research Papers"
category: specialisation
levels: ["senior", "principal"]
skills: [paper-reading, implementation, state-of-the-art]
questions:
  "mid": ["Summarize the key innovation in the 'Attention Is All You Need' paper."]
  "senior": ["Contrast the training objectives of BERT vs GPT-3."]
  "principal": ["Critique the 'Chain of Thought' paper. What are its limitations and how would you improve it?"]
---

# Seminal Research Papers

## 💡 How to Read Papers

Reading papers is a skill. Don't read linearly.
1.  **Pass 1**: Title, Abstract, Figures, Conclusion. (Decide if it's worth reading).
2.  **Pass 2**: Introduction, Method, Results. (Understand the core idea).
3.  **Pass 3**: Math, Appendix, Code. (Deep dive for implementation).
4.  **Pass 4**: Reproducing results.
5.  **Pass 5**: Keeping up with ArXiv / Google Scholars (to stay updated using citations to move forward and backward in time).

### Building a Mental Map (Knowledge Graph)
Don't just collect PDFs. Build a graph of concepts.
*   **Nodes**: Concepts (e.g., "Attention", "Residual Connection", "DPO").
*   **Edges**: Relationships (e.g., "Attention **replaces** RNNs", "DPO **optimizes** RLHF").
*   **Activity**: When reading a new paper, trace its "Ancestors" (what it builds on) and "Descendants" (what improves it).

## 🏆 The "Must Read" List

### Architecture
*   **[Attention Is All You Need (Transformer)](https://arxiv.org/abs/1706.03762)**: The paper that changed everything.
*   **[ResNet (Deep Residual Learning)](https://arxiv.org/abs/1512.03385)**: Solving vanishing gradients in deep networks.
*   **[LoRA: Low-Rank Adaptation](https://arxiv.org/abs/2106.09685)**: Efficient fine-tuning.

### Generative Models
*   **[Denoising Diffusion Probabilistic Models (DDPM)](https://arxiv.org/abs/2006.11239)**: The foundation of modern image generation.
*   **[GPT-3: Language Models are Few-Shot Learners](https://arxiv.org/abs/2005.14165)**: Scaling laws and in-context learning.
*   **[LLaMA: Open and Efficient Foundation Language Models](https://arxiv.org/abs/2302.13971)**: Democratizing LLMs.

### Alignment & Agents
*   **[InstructGPT](https://arxiv.org/abs/2203.02155)**: Using RLHF to follow instructions.
*   **[ReAct: Synergizing Reasoning and Acting in Language Models](https://arxiv.org/abs/2210.03629)**: The basis for Agentic workflows.


## Resources
- [ArXiv Sanity Preserver](http://www.arxiv-sanity.com/)
- [Papers with Code](https://paperswithcode.com/)
- [Google Scholars](https://scholar.google.com/)

## 🛠️ Practice

### Project: Paper to Code
Pick a paper from the list (e.g., LoRA) and try to implement the core logic in PyTorch from scratch, comparing your results to the official implementation.
*   **Goal**: Understand the gap between math notation and code.
*   **Task**: Read and summarize one paper per week.
*   **Task**: Implement a paper's method from scratch.

## 🗣️ Interview Questions

### Mid-Level
*   **Q**: What is "In-Context Learning" as introduced in the GPT-3 paper?
*   **A**: It's the ability of the model to perform a new task just by seeing examples in the prompt, without any gradient updates (fine-tuning).
*   **Q**: What is an ablation study?
*   **A**: It's a method to understand the contribution of each component of a model by removing it and measuring the impact on performance.

### Senior-Level
*   **Q**: Critically analyze a paper's methodology and identify potential flaws.
*   **A**: Analyze the assumptions, limitations, and potential biases in the paper's methodology.
*   **Q**: In the LLaMA paper, what architectural changes did they make compared to the original Transformer?
*   **A**:
    1.  **Pre-normalization** (RMSNorm) for stability.
    2.  **SwiGLU** activation function instead of ReLU.
    3.  **Rotary Embeddings (RoPE)** instead of absolute positional encodings.
*   **Q**: What is the difference between a "vanilla" Transformer and a "scaled dot-product" Transformer?
*   **A**: The "scaled dot-product" Transformer uses a scaled dot-product attention mechanism, which is more stable and allows for longer-range dependencies.

### Principal-Level
*   **Q**: Critique the DPO (Direct Preference Optimization) paper vs standard PPO (Proximal Policy Optimization).
*   **A**: DPO simplifies the RLHF process by treating the preference problem as a classification problem, removing the need for a separate Reward Model and the unstable PPO loop. However, it can be more sensitive to the quality of the preference data and may overfit faster.
