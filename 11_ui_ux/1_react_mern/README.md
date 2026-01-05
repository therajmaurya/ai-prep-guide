---
title: "Frontend Engineering for AI"
category: ui-ux
levels: ["mid", "senior"]
skills: [react, streaming, websockets, tailwind]
questions:
  "mid": ["How do you handle long-running API calls in React?"]
  "senior": ["Implement a 'Typewriter' effect for streaming LLM tokens."]
  "principal": ["Architect a realtime collaborative canvas (like Figma) for Generative AI."]
---

# Frontend Engineering for AI

## 💡 Core Ideas

AI apps feel different. They are slow, unpredictable, and conversational.

*   **Streaming**:
    *   **SSE (Server-Sent Events)**: Standard for LLM streaming one-way.
    *   **WebSockets**: Bi-directional (Voice chat).
*   **State Management**:
    *    Optimistic Updates: Show the message immediately while sending.
*   **Markdown Rendering**:
    *   `react-markdown`: Essential for rendering code blocks and tables from LLMs.

## Components
- Component-based architecture.
- State Management (Redux, Context API).
- Hooks (useEffect, useState).
- Server-side Rendering (Next.js).

## 🛠️ Practice

### Project 1: Chat interface
Build a clone of ChatGPT UI using Next.js.
*   **Goal**: Implement "Stop Generating" button (AbortController).

* Build a dashboard to visualize ML model metrics.
* Implement a real-time chat app using Socket.io.

## 🗣️ Interview Questions

### Mid-Level
*   **Q**: Why not just use `await fetch()` for LLMs?
*   **A**: Users will think the app crashed if they stare at a spinner for 10 seconds. Streaming reduces TBT (Time Between Tokens) to <100ms, making it feel instant.
*   **Q**: Explain the component lifecycle.
*   **A**: 
    *   **Mounting**: `constructor()`, `render()`, `componentDidMount()`.
    *   **Updating**: `render()`, `componentDidUpdate()`.
    *   **Unmounting**: `componentWillUnmount()`.

### Senior-Level
*   **Q**: How do you handle long-running API calls in React?
*   **A**: Use `AbortController` to cancel the request when the component unmounts.
*   **Q**: How does React Fiber work?
*   **A**: TBD
