---
title: "How to Get Started with AI Research"
description: "A comprehensive guide to mastering the art of reading research papers, implementing them, and contributing to the field."
skills: ["research", "paper-reading", "math", "implementation", "career"]
levels: ["mid", "senior"]
links: ["https://www.youtube.com/watch?v=L4NnV_YpS2o&t=2105s"]
---

# How to Get Started with AI Research

This guide outlines a roadmap for transition from a consumer of AI technology to a researcher contributing to the field.

## 1. Foundational Learning

Before diving into complex papers, ensure your foundation is solid.

*   **Machine Learning (ML) & Deep Learning (DL)**: It is crucial to start with solid ML and DL courses. Understanding what happens "under the hood" is important, even as the field matures. Begin with a good ML course and a good DL course (e.g., from a university or high-quality online sources).
*   **Math Fundamentals**: Simultaneously, pay attention to math. At least undergraduate-level math in **linear algebra**, **calculus**, and **probability** is required.
    *   *Note*: It's okay not to master all math at once; you will revisit concepts throughout your career as needed.

## 2. Efficient Paper Reading

After mastering the basics, quickly switch to reading research papers. AI research is too active for courses to keep up; papers are the nodes of new knowledge.

*   **Building a Mental Map**: The goal is to build a "mental map" of the field, enabling you to understand new papers as variations of existing ideas (e.g., "This is just X but with Y modification").
*   **Strategy**:
    *   Gather a list of important papers in a chosen sub-area (e.g., RL, Diffusion, Transformers) from the last 4-5 years. Use survey papers or book chapters as a starting point.
    *   **Quantity**: A rule of thumb is that reading **5-20 papers** on a single topic provides good exposure, while **50 papers** offer deep understanding.
*   **How to Read**:
    *   Don't read linearly like a textbook.
    *   **Start**: Title and Abstract.
    *   **Next**: Main Figures and Experimental Setup.
    *   **End**: Conclusion.
    *   **Deep Dive**: Only go deep into the math and details if the paper truly matters or if you are implementing it.
    *   **Multi-pass reading**: Read paper in multiple passes to develop deep understanding of the paper. With every pass you can increase the depth of details you are looking in the paper. Also, reading multiple times will help with the retention of the paper.
*   **Finding Papers**:
    *   **Backward in time**: Look at citations *within* a paper to find foundational work.
    *   **Forward in time**: Use Google Scholar to see which newer papers *cite* the current one.
*   **Internal Classifier**: Over time, you will develop an "internal classifier" to quickly discern if a paper is a minor tweak, a foundational work, or a new technique.

## 3. Thinking Ahead

While learning a new topic, practice active engagement.

*   **Predict the Next Step**: After reading a couple of chapters or papers, pause and ask yourself: *"What would the next chapter or research step be?"*
*   **Research Mindset**: Even if you are wrong, this practice helps in developing a research mindset and thinking proactively about natural progressions in the field.

## 4. Hands-On Implementation

Bridge the gap between reading and creating by getting hands-on.

*   **Code**: Filter for papers with **public code and datasets**. Download the repositories and run the code.
*   **Experiment**: Don't just run it—modify it.
    *   Try a different dataset.
    *   Tweak the architecture slightly.
    *   Try a new evaluation metric.
*   **Extension over Invention**: The primary skill is often extending and adapting existing research rather than implementing from scratch.
*   **Artifacts**:
    *   **Open Source**: Put your implementations on GitHub. This serves as a portfolio for job applications, PhDs, or internships.
    *   **Blog**: Write blogs to consolidate your thoughts and share your interpretations.

## 5. Math in AI Research

Math is a tool, not a gatekeeper.

*   **Theoretical vs. Empirical**: There are theoretical researchers (proofs/theorems) and empirical researchers (experiments/models). Most applied work is empirical.
*   **Just-in-Time Learning**: Beyond the undergrad basics (linear algebra, calculus, probability), learn math *as specific topics demand*.
    *   If a concept in a paper blocks your understanding, go deep into the math *then*.
    *   Use AI tools (like Gemini/ChatGPT) to explain complex equations or notations.
*   **Intuition**: Knowing the mathematical underpinnings provides intuition, confusing behavior becomes explainable, and you become a more efficient researcher.

## 6. Seeking Mentorship

Moving from self-study to collaboration is a key milestone.

*   **Reach Out**: Contact professors, senior PhD students, or even authors of papers you admire.
*   **Show Your Work**: When reaching out, showcase your public artifacts (GitHub repos, blogs, reproduced results). It shows you are serious and capable.
*   **Role of a Mentor**: Mentors provide guidance on the research process, "research taste" (what problems are worth solving), and how to tell a compelling research story.
*   **Networking**: Hiring in research often happens through recommendations. Look for mentors who actively share knowledge.

## 7. Lifecycle of a Research Project (Empirical)

1.  **Finding a Concrete Problem**: Identify a problem you believe in. This can come from an advisor's nudge or your own observation (e.g., "Why do LLMs fail at X?").
2.  **Deciding Evaluation**: Determine how to quantify the problem. Reusing existing metrics is common, but designing a *new evaluation metric* can itself be a research contribution.
3.  **Surveying Existing Work**: Read papers to see what exists. Identify what is missing or broken. This takes time.
4.  **Formulating a Hypothesis**: Based on insights (non-obvious patterns, failure modes), formulate a testable hypothesis (e.g., "Model fails on X because of Y; changing Z should improve it").
5.  **Designing Experiments**: Plan meaningful experiments to validate or disprove the hypothesis.
6.  **Writing the Paper**: The paper is a *story* around your insight.
    *   **Intro**: What exists, why your work is different.
    *   **Body**: Explanation of your method.
    *   **Experiments**: Evidence supporting your claims.
    *   **Ablations**: Insightful observations on *why* it works (e.g., removing components to see their specific impact).

**Note**: Contributions aren't always new algorithms. They can be new observations, new metrics, negative results (if non-obvious), or new perspectives.

## 8. Career Paths & General Advice

*   **Paths**: After undergraduate research or internships, consider a PhD, Master's, or industry job.
*   **AI Residency**: Excellent bridge between a PhD and an industry job, offering hands-on experience in top labs (Google, Meta, OpenAI, etc.).
*   **The Roadmap**: Fundamentals → Reading Papers & Building Mental Maps → Practical Application (Code/Blog) → Collaborating/Mentoring.
*   **Consistency**: Don't be afraid of the difficulty. Just start, be consistent, and let the compounding effort work wonders.
