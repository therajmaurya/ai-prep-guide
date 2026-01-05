---
title: "Reinforcement Learning"
category: dl
levels: ["senior", "principal"]
skills: [rl, ppo, dqn, offline-rl, mdp]
questions:
  "mid": ["What is the difference between On-Policy and Off-Policy RL?"]
  "senior": ["Explain the Clipped Surrogate Objective in PPO."]
  "principal": ["How do you apply RL to non-differentiable rewards in a real-world recommendation system?"]
---

# Reinforcement Learning

## 💡 Core Ideas

RL is about learning from consequences. It's the key to making AI active (Agents, Robotics, Game Playing).

*   **Markov Decision Process (MDP)**: The mathematical framework ($S, A, P, R, \gamma$).
*   **Value-Based Methods**:
    *   **DQN (Deep Q-Network)**: Learn the Q-function $Q(s, a)$. Uses Replay Buffer and Target Network to stabilize training.
    *   **Double DQN**: Mitigates overestimation bias.
*   **Policy-Based Methods**:
    *   **REINFORCE**: Optimizes the policy $\pi(a|s)$ directly using log-likelihood.
    *   **PPO (Proximal Policy Optimization)**: The standard. Limits how much the policy changes in one step (Trust Region) to prevent collapse. Used in RLHF.
*   **Model-Based RL**:
    *   Learn a model of the world (dynamics), then plan inside (e.g., AlphaZero, Dreamer).
*   **Offline RL**: Learning optimal policies from a static dataset of fixed transitions (vital for healthcare/robotics where exploration is dangerous).

## 📚 Resources

*   [**OpenAI Spinning Up**](https://spinningup.openai.com/) - The absolute best intro to RL.
*   [**Sutton & Barto**](http://incompleteideas.net/book/the-book-2nd.html) - The textbook.
*   [**Deep RL Course (Berkeley CS285)**](http://rail.eecs.berkeley.edu/deeprlcourse/) - Advanced.
*   [Reinforcement Learning: An Introduction (Sutton & Barto)](http://incompleteideas.net/book/the-book-2nd.html)
*   [Spinning Up in Deep RL](https://spinningup.openai.com/)

## 🛠️ Practice

### Project 1: Solve LunarLander
Train a **PPO** agent to solve `LunarLander-v2` (Gymnasium).
*   **Goal**: Achieve average reward > 200. Graph the learning curve.

### Project 2: Offline RL for Trading
Take a dataset of historical stock prices. Train a **DQN** on this "historical" data to trade.
*   **Goal**: Evaluate it on out-of-sample data. Observe how "distribution shift" kills performance (the training data didn't contain the new market regime).

### Project 3: Train an agent to play CartPole using DQN.
### Project 4: Implement Policy Gradient on a simple environment.

## 🗣️ Interview Questions

### Mid-Level
*   **Q**: What is the Exploration-Exploitation trade-off?
*   **A**: Exploration involves taking random actions to discover new states (High Variance). Exploitation involves taking the best-known action to maximize reward (High Bias). Epsilon-Greedy is a simple strategy to balance this.

### Senior-Level
*   **Q**: Why do we need a "Target Network" in DQN?
*   **A**: In DQN, we update $Q(s, a)$ towards $r + \gamma \max Q(s', a')$. If we use the same network to calculate the target, the target keeps moving *with* the update, leading to oscillations (chasing your own tail). Freezing the target network for $N$ steps provides a stable objective.
*   **Q**: What is the difference between On-Policy and Off-Policy RL?
*   **A**: On-Policy methods (e.g., REINFORCE) learn the policy directly from the environment, while Off-Policy methods (e.g., DQN) learn a policy from a replay buffer of past experiences.
*   **Q**: Discuss the challenges of offline RL.
*   **A**: Offline RL is challenging because we don't have access to the environment, only a static dataset of fixed transitions. This makes it hard to learn a policy that generalizes well to new states.

### Principal-Level
*   **Q**: Discuss the challenges of implementing RLHF (Reinforcement Learning from Human Feedback) for LLMs relative to standard RL games.
*   **A**:
    1.  **Reward Sparsity**: There is no explicit reward per token, only per full response.
    2.  **Reward Model Stability**: The RM is itself a learnt neural net and can be "gamed" (Reward Hacking) where the policy finds gibberish that satisfies the RM but not humans.
    3.  **KL Divergence**: We must heavily regularize the policy to stay close to the SFT model, otherwise it forgets language syntax (Mode Collapse).
