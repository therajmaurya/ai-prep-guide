---
title: "Multi-Agent Frameworks"
category: specialisation
levels: ["senior", "principal"]
skills: [autogen, langgraph, crewai, orchestration]
questions:
  "mid": ["Why do we need a framework? Why not just while loops?"]
  "senior": ["Compare AutoGen vs LangGraph. When to use which?"]
  "principal": ["Design a custom orchestration protocol for 1000 agents."]
---

# Multi-Agent Frameworks

## 💡 Core Ideas

Frameworks provide the "glue" for agents: Communication protocols, State Management, and Tool Interfaces.

*   **LangGraph (LangChain)**:
    *   **Philosophy**: **Explicit control**. You define the graph (nodes/edges).
    *   **Best For**: Production apps, strict business logic (Customer Support flows), Human-in-the-loop.
    *   **Key Concept**: The `State` object is shared passed between nodes.
*   **AutoGen (Microsoft)**:
    *   **Philosophy**: **Conversational**. Agents are actors that "speak" to each other.
    *   **Best For**: Exploration, Code generation, "Swarm" behaviors where strict control is less critical.
    *   **Key Concept**: `UserProxy` (executes code/input) <-> `Assistant` (generates code/replies).
*   **CrewAI**:
    *   **Philosophy**: **Role-Playing**. You define "Roles" (Researcher, Writer), "Goals", and "Backstory".
    *   **Best For**: Task delegation pipelines that mimic a human team structure.

## Agentic Design Patterns

*   **Orchestrator-Workers**: A central "Brain" LLM (Orchestrator) breaks a complex plan into subtasks and assigns them to specific "Worker" agents (Math Agent, Search Agent). The Workers return results to the Orchestrator, which synthesizes the final answer. This reduces context clutter for the sub-agents.
*   **Swarm**: A group of agents that work together to solve a problem. Each agent has a specific role and is responsible for a specific task. The agents communicate with each other to coordinate their actions.
*   **Human-in-the-loop**: A human is involved in the decision-making process. The human provides feedback to the agents, which helps them improve their performance.

## Communication Protocols
* MCP
* A2A
* ACP
* AP2

## 📚 Resources

*   [**Building a Multi-Agent Chatbot**](https://langchain-ai.github.io/langgraph/tutorials/multi_agent/multi-agent-collaboration/) - LangGraph Tutorial.
*   [**AutoGen Studio**](https://microsoft.github.io/autogen/docs/Use-Cases/enhanced_inference) - UI for building agents.

## 🛠️ Practice

### Project 1: Coding Team (AutoGen)
Create a 3-agent team:
1.  **Product Manager**: Generates requirements.
2.  **Coder**: Writes Python code.
3.  **Reviewer**: Checks for bugs and security issues.
*   **Goal**: Have them automatically iterate until the Reviewer says "LGTM".

### Project 2: Research Graph (LangGraph)
Create a cyclic graph:
1.  **Search Node**: Google search.
2.  **Writer Node**: Draft answer.
3.  **Critique Node**: Check if answer answers query.
4.  **Loop**: If critique fails, go back to Search with a better query.

## 🗣️ Interview Questions

### Senior-Level
*   **Q**: What is the "Orchestrator-Workers" pattern?
*   **A**: A central "Brain" LLM (Orchestrator) breaks a complex plan into subtasks and assigns them to specific "Worker" agents (Math Agent, Search Agent). The Workers return results to the Orchestrator, which synthesizes the final answer. This reduces context clutter for the sub-agents.
