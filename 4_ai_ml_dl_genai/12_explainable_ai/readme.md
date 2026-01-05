---
title: "Explainable AI (XAI)"
category: must
levels: ["senior", "principal"]
skills: [shap, lime, integrated-gradients, cam, interpretability]
questions:
  "mid": ["Why is interpretability important in regulated industries (Finance/Health)?"]
  "senior": ["Explain the idea behind SHAP values (Game Theory)."]
  "principal": ["Critique 'Saliency Maps'. Are they always reliable?"]
---

# Explainable AI (XAI)

## 💡 Core Ideas

As models get bigger (black boxes), understanding *why* they make a prediction becomes harder but more critical for trust, safety, and debugging.

*   **Global vs Local Interpretability**:
    *   **Global**: Understanding the model as a whole (Feature Importance).
    *   **Local**: Understanding a single prediction (Why was *this* loan rejected?).
*   **Model-Agnostic Methods**:
    *   **LIME (Local Interpretable Model-agnostic Explanations)**: Approximates the complex model locally with a simple linear model.
    *   **SHAP (SHapley Additive exPlanations)**: Based on Cooperative Game Theory. Calculates the marginal contribution of each feature to the output. The only mathematically consistent method.
*   **Model-Specific Methods**:
    *   **Integrated Gradients**: For Neural Networks. Accumulates gradients along the path from a baseline (black image) to the input.
    *   **Grad-CAM**: For CNNs. Visualizes which parts of the image (heatmap) activated the final class.
    *   **Saliency Maps**: For CNNs. Visualizes which parts of the image (heatmap) activated the final class.
*   **Counterfactual Explanations**: Instead of asking "Why Y?", ask "What is the smallest change to X that would change Y to Y'?".
*   **Counterfactual Explanations**: Instead of asking "Why Y?", ask "What is the smallest change to X that would change Y to Y'?".
*   **Concept Bottleneck Models**: Explicitly predicting intermediate human-readable concepts (e.g., "Has Wing" -> "Is Bird") before the final prediction.

### 4. LLM Interpretability (Mechanistic Interpretability)
*   Treating NN as a compilable computer program.
*   **Induction Heads**: Attention heads that look at previous token to predict next token (copying mechanism).
*   **Superposition**: Storing more features than neurons by using non-orthogonal bases.
*   **Dictionary Learning**: Using Sparse Autoencoders (SAE) to disentangle "polysemantic" neurons into single concepts.

## 📚 Resources

*   [**Christoph Molnar's Interpretable ML Book**](https://christophm.github.io/interpretable-ml-book/) - The bible of XAI. Free online.
*   [**Captum**](https://captum.ai/) - PyTorch library for interpretability.
*   [**Distill.pub**](https://distill.pub/) - Beautiful visualizations of neural net internals.

## 🛠️ Practice

### Project 1: Debugging a Classifer with SHAP
Train a Credit Risk model (XGBoost). Use `shap` library to create a Waterfall plot for a specific False Positive.
*   **Goal**: Identify which feature pushed the probability over the threshold.

### Project 2: Visualizing CNNs
Use `Grad-CAM` on a ResNet50 to visualize what it looks at when classifying a "Dog" vs a "Cat".
*   **Goal**: Observe if the model is looking at the animal's face or the background grass (Shortcut Learning).

### Project 3: Counterfactual Explanations
Use `Counterfactual Explanations` to explain why a loan was rejected.
*   **Goal**: Identify which feature pushed the probability over the threshold.

### Project 4: Saliency Maps
Generate saliency maps for a CNN.
*   **Goal**: Observe if the model is looking at the animal's face or the background grass (Shortcut Learning).

## 🗣️ Interview Questions

### Mid-Level
*   **Q**: What is the difference between Feature Importance (e.g., in Random Forest) and SHAP?
*   **A**: Standard Feature Importance (MDI) tells you *global* importance but not direction (does high Age increase or decrease risk?). SHAP gives you *local* importance + direction + magnitude for every single sample.

### Senior-Level
*   **Q**: Explain Counterfactual Explanations.
*   **A**: Instead of asking "Why Y?", ask "What is the smallest change to X that would change Y to Y'?".
    *   *Example*: "Your loan was rejected. If your income was $5k higher, it would have been approved." This is actionable feedback for the user.
*   **Q**: Discuss the limitations of LIME.
*   **A**: LIME is a local explanation method that approximates the complex model locally with a simple linear model. However, it can be sensitive to the choice of baseline and may not capture global patterns. It also assumes that the model is locally linear, which may not be true for complex models.
*   **Q**: What is the difference between SHAP and LIME?
*   **A**: SHAP is a model-agnostic explanation method that calculates the marginal contribution of each feature to the output. It is based on Cooperative Game Theory and is the only mathematically consistent method. LIME is a local explanation method that approximates the complex model locally with a simple linear model.

### Principal-Level
*   **Q**: Are Saliency Maps (like Integrated Gradients) reliable?
*   **A**: Not always. The "Sanity Checks for Saliency Maps" paper showed that many methods act like fancy edge detectors and don't actually depend on the model weights (randomizing weights didn't change the heatmap much). Always validate XAI tools before trusting them.