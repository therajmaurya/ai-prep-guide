---
title: "UI/UX for AI"
category: good-to-know
levels: ["senior", "principal"]
skills: [usability, streaming-interfaces, trust, explainability]
questions:
  "mid": ["Why is latency perception critical in AI applications?"]
  "senior": ["How do you design a UI for a 'Human-in-the-Loop' workflow?"]
  "principal": ["Discuss the ethical implications of 'dark patterns' in AI interfaces."]
---

# UI/UX for AI

## 💡 Core Ideas

Building the model is half the battle. If the user interaction is clunky, the AI feels broken.

*   **Streaming & Latency**: LLMs are slow (Time to First Token matters). Streaming tokens (Server-Sent Events) makes the application feel responsive. Use **skeletons** and **optimistic UI** updates.
*   **Uncertainty & Trust**: UI should communicate confidence. Use color coding or "I don't know" responses instead of hallucinations. Cite sources (citations) where possible.
*   **Control Mechanisms**: Sliders for creativity (Temperature), "Stop Generating" buttons. "Edit" previous user messages to branch conversations.
*   **Feedback Loops**: Thumbs up/down, "Regenerate", and implicit feedback (dwell time, copy to clipboard) to improve the model (RLHF data collection).
*   **Multimodality**: Handling drag-and-drop for images/PDFs, recording voice inputs, and rendering charts/code blocks.

## 🧰 Tools & Libraries

*   **Vercel AI SDK**: The industry standard for building AI-powered user interfaces in React/Next.js/Vue/Svelte. Handles streaming, state, and tool calling abstraction.
*   **Gradio / Streamlit**: Rapid prototyping tools for Python engineers. Good for internal tools, bad for consumer apps.
*   **v0.dev**: Generative UI tool to quickly scaffold frontend components.
*   **Shadcn/UI**: Components often used in modern AI apps (clean, darker aesthetic).

## 📚 Resources

*   [**Google People + AI Guidebook**](https://pair.withgoogle.com/guidebook/) - The gold standard for AI UX design.
*   [**Microsoft HAX Toolkit**](https://www.microsoft.com/en-us/research/project/hax-toolkit/) - Guidelines for Human-AI Interaction.
*   [**Vercel AI SDK Docs**](https://sdk.vercel.ai/docs) - Learn how to implement streaming.

## 🛠️ Practice

### Project 1: Streaming Chat UI
Build a React/Gradio app that streams responses from an OpenAI-compatible API character-by-character.
*   **Goal**: Handle network jitter and Markdown rendering on the fly.

### Project 2: Explainable AI Dashboard
Visualization dashboard for a classification model.
*   **Goal**: Show *why* a decision was made (SHAP values, saliency maps) alongside the prediction.

## 🗣️ Interview Questions

### Mid-Level
*   **Q**: Why should you avoid "magic" in AI interfaces?
*   **A**: Users need mental models of how the system works. If an AI takes an action "magically" without explanation or predictability, users lose trust when it eventually fails.

### Senior-Level
*   **Q**: Design the UX for a Copilot. How do you integrate it without being intrusive?
*   **A**: Use "Ghost text" (gray text ahead of cursor) that can be accepted with Tab. Avoid popups that steal focus. Allow easy dismissal (Esc). implement "debounce" to avoid fetching suggestions on every keystroke.
*   **Q**: How do you handle "Hallucinations" in the UI?
*   **A**: 
    1.  **Citations**: Link to the source document (RAG).
    2.  **Confidence Scores**: Show low confidence warnings.
    3.  **User Feedback**: Easy way to flag incorrect answers.

### Principal-Level
*   **Q**: Determining "Intent". How should the UI adapt when the user's intent is ambiguous?
*   **A**: The UI should ask clarifying questions (disambiguation) rather than guessing. Alternatively, offer multiple variations of the output for the user to select (generating 3 images instead of 1).
