# Contributing to AI Prep Guide

Thank you for your interest in contributing! This guide details how to structure your contributions to ensure high quality and consistency.

## Core Principles

1.  **Structured Metadata**: Every topic must have YAML frontmatter.
2.  **Experience Levels**: Content should be tagged with appropriate experience levels (`mid`, `senior`, `principal`).
3.  **Actionable**: Focus on "Core Ideas", "Resources", and "Practice".

## Topic File Structure

Every topic file (e.g., `README.md` in a topic folder) must start with this frontmatter:

```yaml
---
title: "Topic Name"
category: must  # or good-to-know, specialisation
levels: ["mid", "senior", "principal"] # Select applicable
skills: [skill-1, skill-2]
questions:
  "mid": ["Example mid-level question?"]
  "senior": ["Example senior-level question?"]
  "principal": ["Example principal-level question?"]
---
```

### Sections

After the frontmatter, include the following sections:

```markdown
# Topic Name

## Core Ideas
- Key concept 1
- Key concept 2

## Resources
- [Resource Name](url) - Brief description

## Practice
- Exercise 1
- Exercise 2

## Questions
### Mid Level
- Question...

### Senior Level
- Question...
```

## Pull Request Process

1.  Fork the repository.
2.  Create a branch: `topic/your-topic-name`.
3.  Add your content following the structure above.
4.  Open a PR and tag maintainers.

## Style Guide

- Use clear, concise English.
- Prefer bullet points over long paragraphs.
- Link to official documentation where possible.