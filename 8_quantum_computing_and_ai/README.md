---
title: "Quantum Computing & AI"
category: specialisation
levels: ["senior", "principal"]
skills: [qiskit, pennylane, vqe, qgan, quantum-kernels]
questions:
  "mid": ["What is a Qubit and how does it differ from a classical bit?"]
  "senior": ["Explain the concept of Quantum Machine Learning (QML) and Variational Quantum Eigensolvers (VQE)."]
  "principal": ["Discuss the potential advantages and current limitations of Quantum Neural Networks compared to Classical NNs."]
---

# Quantum Computing & AI

## 💡 Core Ideas

Quantum AI is a burgeoning field exploring how quantum mechanics can accelerate or enhance machine learning algorithms. While still in its infancy, it holds promise for solving optimization problems and simulating molecular structures.

*   **Quantum Basics**:
    *   **Superposition**: A qubit can be in a state of |0⟩, |1⟩, or both simultaneously.
    *   **Entanglement**: Correlated states between qubits, independent of distance.
    *   **Interference**: Amplifying correct solutions and canceling incorrect ones.
*   **Quantum ML Algorithms**:
    *   **Variational Quantum Eigensolver (VQE)**: Hybrid quantum-classical algorithm for finding eigenvalues (useful in chemistry/materials).
    *   **Quantum Approximate Optimization Algorithm (QAOA)**: For combinatorial optimization.
    *   **Quantum Neural Networks (QNN)**: Parameterized quantum circuits optimized via classical gradient descent.
    *   **Quantum SVM**: Using quantum kernels to map data into high-dimensional Hilbert space.

## 📚 Resources

*   [**Qiskit Textbook**](https://qiskit.org/textbook/preface.html) - IBM's interactive guide to Quantum Computing.
*   [**PennyLane**](https://pennylane.ai/) - A library for extensive Quantum Machine Learning.
*   [**Quantum Machine Learning Course**](https://www.edx.org/course/quantum-machine-learning) - Introduction by University of Toronto.

## 🛠️ Practice

### Project 1: Quantum Teleportation
Implement the Quantum Teleportation protocol using Qiskit.
*   **Goal**: Understand quantum circuits, gates (Hadamard, CNOT), and measurement.

### Project 2: Variational Classifier
Build a simple binary classifier using a Variational Quantum Circuit in PennyLane.
*   **Goal**: Train the circuit to classify the Iris dataset (reduced to 2 classes/features) and compare with classical Logistic Regression.

## 🗣️ Interview Questions

### Mid-Level
*   **Q**: What is "Quantum Supremacy"?
*   **A**: The point where a quantum computer can perform a calculation that is practically impossible for a classical supercomputer to perform in a reasonable timeframe.

### Senior-Level
*   **Q**: How do Parameterized Quantum Circuits (PQCs) function in QML?
*   **A**: PQCs act as the "model". They have adjustable parameters (rotation angles of gates). We measure the output, calculate a loss, and use a classical optimizer (like L-BFGS or Adam) to update the parameters, similar to training a neural network.

### Principal-Level
*   **Q**: What are the main challenges in scaling Quantum AI today?
*   **A**:
    1.  **Noise/De-coherence**: NISQ (Noisy Intermediate-Scale Quantum) devices are error-prone.
    2.  **Qubit Count**: Current devices have limited qubits (10s-100s), limiting the size of problems.
    3.  **Data Loading**: Encoding classical data into quantum states is expensive ("The Input Problem").
