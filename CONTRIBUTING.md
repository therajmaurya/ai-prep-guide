# Contributing to AI Prep Guide

First off, thanks for taking the time to contribute! :tada:

The goal of this repository is to create the most comprehensive, structured, and high-quality preparation guide for Senior+ AI/ML engineers. We value your expertise and help.

## Code of Conduct

This project and everyone participating in it is governed by the [Code of Conduct](CODE_OF_CONDUCT.md). By participating, you are expected to uphold this code.

## How Can I Contribute?

### Reporting Bugs
This section guides you through submitting a bug report for the AI Prep Guide. Following these guidelines helps maintainers and the community understand your report, reproduce the behavior, and find related reports.

*   **Use a clear and descriptive title** for the issue to identify the problem.
*   **Describe the exact steps which reproduce the problem** in as much detail as possible.
*   **Explain which behavior you expected to see instead and why.**

### Suggesting Enhancements
This section guides you through submitting an enhancement suggestion for the AI Prep Guide, including completely new topics and minor improvements to existing functionality.

*   **Use a clear and descriptive title** for the issue to identify the suggestion.
*   **Provide a step-by-step description of the suggested enhancement** in as much detail as possible.
*   **Explain why this enhancement would be useful** to most AI Prep Guide users.

### Pull Requests

The process is straightforward:

1.  **Fork** the repo on GitHub.
2.  **Clone** the project to your own machine.
3.  **Create a branch** for your edit/addition: `git checkout -b topic/my-new-topic`.
4.  **Commit** changes to your own branch.
5.  **Push** your work back to your fork.
6.  **Submit a Pull Request** so that we can review your changes.

## Taxonomy & Definitions

To keep topics consistent and searchable, we use strict definitions for **Categories** and **Experience Levels**.

### Category Meanings
*   **must**: Core knowledge every practitioner working in that area *should* have (e.g., linear algebra basics, training loop mechanics).
*   **good-to-know**: Helpful extensions or common practices that make engineers more effective but are not strictly required (e.g., advanced regularization, specific data augmentation).
*   **specialisation**: Deep, domain-specific, or role-specific knowledge (e.g., few-shot prompting research, model parallelism internals).

### Experience-Level Expectations
Use these as guidance when tagging `levels` and authoring questions:

*   **mid** (~5y exp): Solid practitioner. Can implement end-to-end models, debug training, and write production code.
    *   *Focus*: Coding, model internals, evaluation, data cleaning.
*   **senior** (~10y exp): Owns design/delivery of full ML systems. Makes tradeoffs across accuracy, latency, cost.
    *   *Focus*: System design, experiment design/rollout, mentoring, reproducible research.
*   **principal** (15y+ exp): Strategy, system-of-systems design, risk/regulatory decisions, org-level roadmaps.
    *   *Focus*: Platform strategy, compliance, cross-team MLOps, shaping research directions.

## Content Standards & Style Guide

### Topic Structure (The "Golden Rule")

Every topic `README.md` **MUST** begin with the following YAML frontmatter:

```yaml
---
title: "Topic Name"
category: must  # Options: must, good-to-know, specialisation
levels: ["mid", "senior", "principal"] # Select all that apply
skills: [skill-keyword-1, skill-keyword-2]
questions:
  "mid": ["Example question suitable for mid-level?"]
  "senior": ["Example question suitable for senior-level?"]
  "principal": ["Example question suitable for principal-level?"]
---
```

### Markdown Style

*   **Headings**: Use `#` for the main title (only one per file), `##` for major sections, `###` for subsections.
*   **Lists**: Use hyphens `-` for unordered lists and numbers `1.` for ordered lists.
*   **Code**: Use **fenced code blocks** with language headers for all code snippets.
    ```python
    def example():
        return "Like this!"
    ```
*   **Links**: Use relative links for internal navigation `[link text](../folder/file.md)`.

### New Topic Checklist

When adding a new topic, ensure you include:
1.  [ ] **Core Ideas**: Bullet points summarizing the key concepts.
2.  [ ] **Resources**: 3-5 high-quality links (papers, blogs, courses).
3.  [ ] **Practice**: Concrete exercises or mini-projects.
4.  [ ] **Questions**: At least one interview question per experience level.

## Community

If you have questions, feel free to open a [Discussion](https://github.com/therajmaurya/ai-prep-guide/discussions) (if enabled) or an Issue with the `question` label.