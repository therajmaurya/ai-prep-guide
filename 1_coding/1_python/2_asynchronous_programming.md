---
title: "Asynchronous Python"
category: coding
levels: ["mid", "senior", "principal"]
skills: [asyncio, concurrency, event-loop, aiohttp]
questions:
  "mid": ["Difference between threading and asyncio?", "What is the Event Loop?"]
  "senior": ["How do you limit concurrency (throttling) in asyncio?", "Explain `asyncio.all_tasks()` and task cancellation."]
  "principal": ["Design a robust distributed crawler using asyncio and queues.", "How to mix sync and async code safely?"]
---

# Asynchronous Programming in Python

AsyncIO is critical for AI engineers because modern AI apps are **I/O bound** (waiting for GPUs, DBs, or API responses).

## 💡 Core Concepts

### 1. The Event Loop
The central execution device. It runs one task at a time and switches when a task waits (`await`).
*   **Single-threaded**: No race conditions on memory (mostly).
*   **Cooperative Multitasking**: Tasks must voluntarily yield control.

### 2. Coroutines (`async/await`)
*   **`async def`**: Defines a coroutine. Calling it returns a coroutine object, it doesn't run it.
*   **`await`**: Pauses the coroutine until the awaited thing is done. Yields control back to the loop.

```python
import asyncio

async def fetch_data():
    print("Start fetching")
    await asyncio.sleep(1) # Simulates I/O (Database, API)
    print("Done fetching")
    return {"data": 123}

async def main():
    result = await fetch_data()
    print(result)

if __name__ == "__main__":
    asyncio.run(main())
```

### 3. Running Concurrently (`gather`)
To run things in parallel (interleaved), use `asyncio.gather`.

```python
async def main():
    # Runs both fetches "simultaneously"
    results = await asyncio.gather(fetch_data(), fetch_data())
    print(results)
```

## 🚀 Advanced Patterns for AI

### 1. Throttling (Semaphores)
When calling LLM APIs, you often hit rate limits. Use a Semaphore to limit concurrent requests.

```python
sem = asyncio.Semaphore(10) # Max 10 concurrent requests

async def safe_request(url):
    async with sem:
        return await client.get(url)
```

### 2. `as_completed` for Streaming
Process results as soon as they arrive, rather than waiting for all to finish.

```python
tasks = [fetch_data(i) for i in range(5)]
for completed_task in asyncio.as_completed(tasks):
    result = await completed_task
    print(f"Processed {result}")
```

### 3. Blocking Code in Async
If you have a CPU-heavy function (like image resizing or tokenization) inside an async function, it **blocks the loop**. Offload it to a thread.

```python
def heavy_cpu_task():
    # heavy math
    return 42

async def main():
    loop = asyncio.get_running_loop()
    # Run in a separate thread, non-blocking
    result = await loop.run_in_executor(None, heavy_cpu_task)
```

## 🛠️ Practice

*   **Project 1: Async Web Crawler**: Build a scraper that crawls 100 pages using `aiohttp` and extracts text.
*   **Project 2: LLM Batch Processor**: A script that takes 1000 prompts, sends them to OpenAI API with a concurrency limit of 50, and saves results to a JSONL file.

## ❓ Questions

### Mid Level
*   **Q:** `time.sleep()` vs `asyncio.sleep()`?
*   **A:** `time.sleep()` blocks the entire thread (freezes the app). `asyncio.sleep()` yields control, letting other tasks run.

### Senior Level
*   **Q:** What happens if an exception occurs in `asyncio.gather`?
*   **A:** By default, it propagates immediately and other tasks are cancelled (or left running depending on implementation details). Use `return_exceptions=True` to get exceptions as values in the result list.

### Principal Level
*   **Q:** How does `uvloop` make asyncio faster?
*   **A:** It's a drop-in replacement for the standard event loop, implemented in Cython/C on top of `libuv` (same as Node.js). It significantly reduces the overhead of the loop itself.
