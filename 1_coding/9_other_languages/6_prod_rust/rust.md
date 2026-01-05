---
title: "Rust for AI"
category: languages
levels: ["mid", "senior"]
skills: [rust, candle, tch-rs, pyo3, memory-safety]
questions:
  "mid": ["Why use Rust over Python for certain AI components?"]
  "senior": ["Explain how PyO3 allows calling Rust from Python.", "What is the difference between RefCell and Mutex in Rust?"]
  "principal": ["Design a high-performance inference sidecar using Rust."]
---

# Rust for AI Engineering

## 💡 Core Ideas

Rust is becoming the language of choice for **AI Infrastructure** (Vector DBs like Qdrant/LanceDB, Serving engines like Text-Generation-Inference).

*   **Memory Safety**: No Garbage Collector (GC) pauses (unlike Java/Go), no Segfaults (unlike C++).
*   **Performance**: Comparable to C++.
*   **Interoperability**: First-class Python bindings via `PyO3` and `Maturin`. You can write a critical loop in Rust and call it from Python as a native module.

## 📚 Key Libraries

*   [**Candle**](https://github.com/huggingface/candle): Minimalist ML framework by Hugging Face. Logic is similar to PyTorch but in Rust. Supports Llama, Whisper, etc.
*   [**Burn**](https://burn.dev/): Dynamic graph ML framework.
*   [**Tch-rs**](https://github.com/LaurentMazare/tch-rs): Rust bindings for LibTorch (PyTorch C++).
*   [**Polars**](https://pola.rs/): DataFrame library (Pandas alternative) written in Rust. Ultra-fast.

## 🛠️ Practice

### Project 1: Rust Tokenizer
Write a BPE (Byte Pair Encoding) tokenizer in Rust.
*   **Goal**: Expose it to Python using `PyO3`. Compare speed with pure Python implementation.

### Project 2: Inference with Candle
Load a quantized Llama-2 model using `Candle` and run inference on CPU.
*   **Goal**: Create a small CLI chat interface.

## 🗣️ Interview Questions

### Senior-Level
*   **Q**: How does Rust handle concurrency differently from Python?
*   **A**: Python uses a Global Interpreter Lock (GIL), limiting real parallelism. Rust has "Fearless Concurrency". Data races are caught at compile time. You can use `Rayon` to parallelize iterators with one line of code: `.par_iter()`.
