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
    *   **Vercel AI SDK (`useChat`, `useCompletion`)**: Abstracts away the complexity of decoding the stream and updating React state.
    *   **WebSockets**: Bi-directional (Voice chat, Realtime updates).
*   **State Management**:
    *   **Optimistic Updates**: Show the user's message immediately while sending.
    *   **Preserving State**: Keeping chat history alive when navigating away (Persisting to `localStorage` or `IndexedDB`).
*   **Markdown Rendering**:
    *   `react-markdown` + `remark-gfm`: Essential for rendering code blocks, tables, and LaTeX from LLMs.
    *   **Syntax Highlighting**: preventing layout shifts (CLS) when code blocks load.

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
*   **A**: React Fiber is the reconciliation engine. It breaks rendering work into chunks (units of work) so it can pause and yield back to the main thread (Time Slicing). This is crucial for keeping high-priority updates (like typing) responsive while rendering low-priority updates (like a large incoming data chart) in the background. It uses a singly linked list tree traversal.
*   **Q**: React Server Components (RSC) vs Client Components for AI apps?
*   **A**: 
    *   **RSC**: Use for fetching the initial prompt data, checking auth, or database calls.
    *   **Client**: Use for the Chat input, the scrolling list of messages, and anywhere you need standard `useState`/`useEffect` hooks for the streaming response.
