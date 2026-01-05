---
title: "Agentic AI Systems"
category: specialisation
levels: ["senior", "principal"]
skills: [multi-agent, planning, tool-use, rag, frameworks]
questions:
  "mid": ["What is the ReAct loop in the context of Agents?"]
  "senior": ["Architect a multi-agent coding assistant (Planner, Coder, Reviewer). How do they communicate?"]
  "principal": ["Discuss the safety and control challenges of autonomous agents acting on the internet."]
---

# Agentic AI Systems

## 💡 Core Ideas

LLMs that just "chat" are passive. Agents are "active"—they can plan, use tools, and interact with the world to solve open-ended goals.

*   **Architectures**:
    *   **Single Agent**: ReAct (Reason + Act), Plan-and-Solve.
    *   **Multi-Agent**: Orchestrator-Worker, Hierarchical Teams, Debate.
*   **Components**:
    *   **Planning**: Decomposing complex goals into steps (Chain of Thought, Tree of Thoughts).
    *   **Memory**: Short-term (context window) vs Long-term (Vector DB).
    *   **Tools**: Function Calling (OpenAI), API integration.
*   **Frameworks**: LangChain, AutoGen, CrewAI.

## 📚 Resources

*   [**Everything You Need to Know About Agents**](https://lilianweng.github.io/posts/2023-06-23-agent/) - Lilian Weng's seminal blog post.
*   [**AutoGen Paper**](https://arxiv.org/abs/2308.08155) - Microsoft's multi-agent framework.
*   [**Voyager**](https://voyager.minedojo.org/) - An open-ended embodied agent with large language models.

## 🛠️ Practice

### Project 1: Research Assistant
Build an agent using `LangGraph` that can:
1.  Verify a user's query.
2.  Search the web using Tavily/SerpAPI.
3.  Summarize findings into a markdown report.

### Project 2: Multi-Agent Debate
Use `AutoGen` to create two agents (e.g., "Optimist" and "Pessimist") who debate a stock's future. A third "Judge" agent decides the winner.

## 🗣️ Interview Questions

### Mid-Level
*   **Q**: How does an LLM "call a function"?
*   **A**: The LLM doesn't execute code. It outputs a structured JSON (or XML) containing the function name and arguments. The runtime environment (your Python script) parses this, executes the actual code (e.g., query_database), and feeds the result back to the LLM as a new message.

### Senior-Level
*   **Q**: What is the "Context Window" problem in long-running agent tasks, and how do you mitigate it?
*   **A**: Agents accumulate history (observations, errors, thoughts) which fills the context window quickly. Mitigations:
    1.  **Summarization**: Periodically summarize older history.
    2.  **Vector Memory**: Store raw logs in a Vector DB and retrieve only relevant snippets (RAG).
    3.  **Scratchpad**: Keep a separate "current state" object that is updated, rather than re-reading all history.

### Principal-Level
*   **Q**: Design a secure sandbox for an Agent that can write and execute Python code.
*   **A**:
    1.  **Isolation**: Run code in a momentary Docker container (with no network or restricted network) or use gVisor/Firecracker microVMs.
    2.  **Resource Limits**: Strict CPU/RAM/Time limits to prevent infinite loops (DoS).
    3.  **Static Analysis**: Pre-scan generated code for malicious imports (e.g., `os.system`, `subprocess`).
