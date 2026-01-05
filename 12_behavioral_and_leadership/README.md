---
title: "Behavioral & Leadership"
category: must
levels: ["senior", "principal"]
skills: [mentorship, strategy, stakeholder-management, ethics, hiring]
questions:
  "mid": ["Tell me about a time you disagreed with a PM about a model metric."]
  "senior": ["How do you convince leadership to invest in MLOps infrastructure over new features?"]
  "principal": ["How do you define the AI strategy for a company that has no prior AI experience?"]
---

# Behavioral & Leadership

## 💡 Core Ideas

As a Senior+ Engineer, your code is only 30% of your impact. The rest is influence, strategy, and people.

*   **Mentorship**: Elevating junior engineers. Code reviews are teaching moments, not just gatekeeping.
*   **Strategy**: "Build vs Buy". Knowing when to use an API (OpenAI) vs train a custom model (Llama) is the #1 strategic decision today.
*   **Ethics & Safety**: Responsible for bias, hallucinations, and deepfakes.
*   **Stakeholder Management**: Translating "F1 Score" and "Perplexity" into "Revenue" and "Customer Satisfaction" for business stakeholders.

## Aspects:
- STAR Method (Situation, Task, Action, Result).
- Conflict Resolution.
- Mentorship and Team Building.
- Prioritization and Trade-offs.

## 📚 Resources

*   [**Staff Engineer: Leadership beyond the management track**](https://staffeng.com/) - Will Larson's book.
*   [**The Manager's Path**](https://www.oreilly.com/library/view/the-managers-path/9781491973882/) - Useful even for ICs to understand their managers.

## 🛠️ Practice

### Project: Write a RFC (Request for Comment)
Write a design doc proposing a major architectural change (e.g., "Migration from AWS SageMaker to Custom K8s Cluster").
*   **Goal**: Outline "Background", "Proposed Solution", "Alternatives Considered", "Cost Analysis", and "Risks".

### Project: Prepare 5 STAR stories.
### Project: Conduct a mock behavioral interview.

## 🗣️ Interview Questions

### Senior-Level
*   **Q**: A junior engineer is stuck on a bug for 2 days. What do you do?
*   **A**: Don't fix it for them.
    1.  Ask them to explain their debugging process so far.
    2.  Guide them with questions ("Have you checked the input data distribution?").
    3.  Pair program for 30 mins to unblock, then step back.
*   **Q**: How do you handle tight deadlines?
*   **A**: 
    1.  **Prioritize**: Identify critical dependencies and dependencies with high risk of failure.
    2.  **Communicate**: Keep stakeholders informed about progress and potential roadblocks.
    3.  **Delegate**: Assign tasks to team members based on their expertise and availability.
    4.  **Review**: Regularly review progress and adjust plans as needed.
*   **Q**: Describe a time you had to influence without authority.
*   **A**: 
    1.  **Identify**: Identify the key stakeholders and their interests.
    2.  **Communicate**: Clearly communicate the benefits and risks of the proposed change.
    3.  **Negotiate**: Be open to compromise and be willing to listen to their concerns.
    4.  **Follow up**: Follow up with stakeholders to ensure they are satisfied with the outcome.

### Principal-Level
*   **Q**: You discover that the model regulating credit limits is biased against a certain demographic. The product launch is tomorrow. What do you do?
*   **A**:
    1.  **Stop the launch**. Ethics/Legal risk > Short term deadline.
    2.  **Investigate**: Use bias audit tools (Fairlearn).
    3.  **Mitigate**: Re-sample data, re-train, or adjust thresholds.
    4.  **Communicate**: Explain to stakeholders *why* this prevents a PR disaster and potential lawsuit.
