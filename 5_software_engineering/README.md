---
title: "Software Engineering for AI"
category: must
levels: ["mid", "senior", "principal"]
skills: [clean-code, testing, design-patterns, refactoring, debugging]
questions:
  "mid": ["How do you unit test a stochastic machine learning model?"]
  "senior": ["Explain the 'Adapter' pattern and how it might be used when switching between different LLM providers."]
  "principal": ["How do you manage technical debt in a rapidly evolving ML codebase (prototyping vs production)?"]
---

# Software Engineering for AI

## 💡 Core Ideas

Writing "research code" (Jupyter notebooks) is very different from writing "production code". Bridging this gap is the #1 skill for Senior ML Engineers.

*   **Clean Code for ML**: Type hinting (`typing`), docstrings, modularity (breaking giant scripts into functions/classes), and dependency management (Poetry/Conda).
*   **Testing Strategies**:
    *   **Unit Tests**: Testing individual functions (e.g., data loading logic).
    *   **Integration Tests**: Checking if the model pipeline runs end-to-end.
    *   **Regression Tests**: Ensuring model performance hasn't degraded.
    *   **Property-Based Testing**: Checking invariants (e.g., probability output sum is always 1.0).
*   **Design Patterns for ML**:
    *   **Strategy Pattern**: Swapping algorithms (e.g., `GreedyDecoder` vs `BeamSearchDecoder`) at runtime.
    *   **Factory Pattern**: Creating models from config string (`ModelFactory.create("resnet50")`).
    *   **Adapter Pattern**: Standardizing API for different LLM providers (OpenAI, Anthropic, Local) -> `llm.generate()`.
    *   **Observer Pattern**: Logging metrics during training (Callbacks).
*   **Refactoring**: Moving from Notebook -> Script -> Package -> Microservice.

## 📚 Resources

*   **[Clean Code in Python](https://www.packtpub.com/product/clean-code-in-python-second-edition/9781800560215)** - Essential for avoiding spaghetti code.
*   **[Testing Machine Learning Systems](https://eugeneyan.com/writing/testing-ml/)** - Great blog post by Eugene Yan.
*   **[Refactoring Guru](https://refactoring.guru/design-patterns)** - General design patterns guide.

## 🛠️ Practice

### Project 1: Refactor a Notebook
Take a messy Kaggle notebook (spaghetti code, global variables) and refactor it into a clean Python package structure with `src/`, `tests/`, and `pyproject.toml`.
*   **Goal**: Ensure it can be run with a single command (e.g., `python -m my_model.train`).

### Project 2: Robust Model Testing
Write a test suite for a text classification model using `pytest`.
*   **Goal**: Includes tests for input shapes, output ranges (0-1 for probability), and handling of edge cases (empty strings).

## 🗣️ Interview Questions

### Mid-Level
*   **Q**: Why should you avoid global variables in your training script?
*   **A**: They make testing difficult, introduce side effects, and make it hard to run parallel experiments or integrate the code into other systems.

### Senior-Level
*   **Q**: How do you test a function that involves a random number generator?
*   **A**: You can seed the RNG (`torch.manual_seed(42)`) to make it deterministic for the test. Alternatively, use statistical tests (like Kolmogorov-Smirnov) to verify that the output distribution matches expectation over many runs, or mock the RNG.

### Principal-Level
*   **Q**: We have a monolithic training repo that 50 researchers contribute to. Builds are slow, and breaking changes happen daily. How do you fix this?
*   **A**:
    1.  **Modularization**: Split core infrastructure (training loops, loggers) from experimental modeling code.
    2.  **CI/CD**: Implement strict pre-commit hooks (linting, formatting) and run tests on modified paths only (using tools like `pytest-testmon` or Bazel).
    3.  **Governance**: Enforce code owners for core paths.
    4.  **Versioning**: Version control data and model artifacts, not just code.
