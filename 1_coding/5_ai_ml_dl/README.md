---
title: "AI/ML/DL Coding Patterns"
category: must
levels: ["mid", "senior"]
skills: [coding-patterns, training-loops, inference]
questions:
  "mid": ["Write a basic training loop in PyTorch."]
  "senior": ["Implement a custom data loader with pre-fetching."]
---

# AI/ML/DL Coding Patterns

## Core Ideas
- Standard training loops (Forward, Loss, Backward, Step).
- Checkpointing and saving/loading models.
- Inference pipelines.
- Metric calculation during training.

## Resources
- [PyTorch Examples](https://github.com/pytorch/examples)
- [Keras Code Examples](https://keras.io/examples/)

## Practice
- Write a reusable training loop class.
- Implement early stopping logic.

## Questions
### Mid Level
- How to handle gradient accumulation?
- What is the purpose of `model.eval()`?

### Senior Level
- Design a plugin-based trainer that supports callbacks.
