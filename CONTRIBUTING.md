# Contributing to ai-prep-guide

Thank you for considering contributing. This document describes the contribution rules, frontmatter schema, PR checklist, and an explicit mapping of experience levels used across this repo.

IMPORTANT: this repository uses `levels` in topic frontmatter (not numeric year tags). Mapping used in this repo:

- `mid` ≈ mid-level contributor (~5 years experience)
- `senior` ≈ senior / lead engineer (~10 years experience)
- `principal` ≈ principal / architect / executive technical leader (15+ years)

Treat the mapping as a shorthand only — use the label that best describes the expected knowledge level.

---

Frontmatter schema (required)

Every topic README must include YAML frontmatter at the top with the following fields:

```yaml
---
title: "Human friendly title"
category: must | good-to-know | specialisation
levels: ["mid","senior","principal"]  # one or more
skills: [keyword1, keyword2]
questions:
  "mid": ["example question 1", "example question 2"]
  "senior": ["example question for senior"]
  "principal": ["example question for principal"]
---
```

Notes:
- `levels` must use the literal tags `mid`, `senior`, or `principal`.
- Provide at least one example question for each level you list.
- Keep `skills` short keyword tokens (no long sentences).

File naming and placement

- Put topic files under the appropriate top-level folder (for example `4_ai_ml_dl_genai/3_deep_learning/transformers/README.md`).
- Filenames should be lowercase with underscores for word separators (e.g., `fine_tuning.md`).
- Avoid extremely deep nesting unless the topic truly requires it.

Before opening a PR

1. Add or update the topic file under the proper folder.
2. Include YAML frontmatter using the schema above and set `category`, `levels`, and `skills`.
3. Write a short summary (3–10 lines), a small `Resources` list (3–6 entries), and 1–3 `Practice` items.
4. Add `questions` examples mapped to each `levels` tag you declared.
5. In the PR description include a one-line rationale for the chosen `category` and `levels`.

PR checklist (what reviewers will look for)

- Frontmatter fields present and valid (`title`, `category`, `levels`, `skills`, `questions`).
- Content is concise, accurate, and non-sensitive (no secrets/PII).
- Category assignment is justified in the PR description.
- At least one example question per listed level.
- No large pasted copyrighted text (link to papers instead).

Labels & branch naming

- Branch naming: `topic/<folder>/<short-name>` or `fix/meta/<short>` for metadata-only changes.
- Suggested labels: `area:content`, `area:meta`, `category:must`, `category:good-to-know`, `category:specialisation`, `status:needs-review`, `status:approved`.

Review and merge process

- Metadata-only PRs (frontmatter or small fixes): fast review (1–2 business days).
- New topic content: substantive review (2–5 business days). Reviewers will check the category and level assignments carefully.

If you need help

- Open an issue with the `categorize:` or `add:` prefix and include a short rationale.
- For frontmatter validation help, request a review from maintainers and tag `@maintainers` if available.

Automation (optional next-step)

We recommend adding a small CI validation check that ensures each topic README has the required frontmatter keys and that `levels` contains only allowed values. If you want, open an issue titled `proposal: add frontmatter validator` and I can draft a small Python script and GitHub Action for it.

Thank you — contributions make this repo better for everyone.