---
title: "Python for AI/ML"
category: coding
levels:
  - "mid"
  - "senior"
  - "principal"
skills:
  - "python"
  - "data-structures"
  - "algorithms"
  - "performance-optimization"
  - "concurrency"
  - "internals"
questions:
  mid:
    - "Explain list comprehensions vs generators."
    - "What is a decorator?"
    - "Difference between `is` and `==`."
  senior:
    - "How does Python's GIL affect multi-threaded ML data loading?"
    - "Explain the difference between `__new__` and `__init__`."
    - "How does garbage collection work in Python?"
  principal:
    - "Design a plugin system using Python's dynamic features."
    - "Optimize a memory-intensive Python application."
    - "Compare `multiprocessing` vs `asyncio` for an ML inference service."
---

# Python for AI/ML Interviews

Python is the lingua franca of AI/ML. While it's easy to learn, mastering it requires understanding its internal mechanics, memory management, and concurrency models. Interviews for AI roles focus heavily on writing efficient, "Pythonic" code and understanding how Python interacts with low-level libraries (like PyTorch/TensorFlow).

## Core Concepts

### 1. Data Structures & Complexity
Understanding the time complexity of built-in data structures is non-negotiable.

*   **Lists (`list`)**: Dynamic arrays.
    *   Append/Pop (end): $O(1)$
    *   Insert/Delete (middle): $O(N)$
    *   Get/Set Item: $O(1)$
*   **Dictionaries (`dict`)**: Hash maps.
    *   Get/Set/Delete: $O(1)$ average, $O(N)$ worst case (collisions).
    *   *Note*: Since Python 3.7+, dicts preserve insertion order.
*   **Sets (`set`)**: Hash sets (keys only).
    *   Add/Remove/Check: $O(1)$ average.
    *   Set operations (Union, Intersection): $O(len(s) + len(t))$.
*   **Tuples (`tuple`)**: Immutable lists.
    *   More memory efficient than lists. Used for dictionary keys (if elements are hashable).

### 2. Iterators & Generators
Efficient data processing often requires lazy evaluation to avoid loading datasets entirely into RAM.

*   **Iterator Protocol**: An object is an iterator if it implements `__iter__` (returns self) and `__next__` (returns next value or raises `StopIteration`).
*   **Generator Function**: Uses `yield`. When called, returns a generator object. State is suspended between yields.
*   **Generator Expression**: `(x*x for x in range(10))` - Memory efficient alternative to list comprehensions.
*   **`yield from`**: Delegates to a sub-generator. Useful for recursive generators or refactoring.

```python
def deep_flatten(iterable):
    for item in iterable:
        if isinstance(item, (list, tuple)):
            yield from deep_flatten(item)
        else:
            yield item
```

### 3. Decorators
Decorators are higher-order functions that modify other functions or classes.

*   **Basic**: Wraps a function to add logic (logging, timing).
*   **`functools.wraps`**: Crucial to preserve the metadata (name, docstring) of the decorated function.
*   **Decorators with Arguments**: Requires three levels of nested functions.
*   **Class Decorators**: modifying a class definition (e.g., adding methods, registering to a registry).

```python
import functools
import time

def timer(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        start = time.perf_counter()
        result = func(*args, **kwargs)
        print(f"{func.__name__} took {time.perf_counter() - start:.4f}s")
        return result
    return wrapper
```

### 4. Object-Oriented Programming (OOP)
*   **Magic Methods (Dunder Methods)**:
    *   `__init__`: Initializer.
    *   `__new__`: Constructor (creates the instance). Used in singletons or subclassing immutable types.
    *   `__call__`: Makes an instance callable like a function.
    *   `__getitem__`, `__len__`: For sequence behavior.
*   **MRO (Method Resolution Order)**: Python uses the C3 Linearization algorithm to determine the order in which base classes are searched. View it via `ClassName.mro()`.
*   **Slots**: `__slots__` restricts valid attributes to a fixed set, saving memory by suppressing `__dict__` creation.

## Advanced Topics

### 1. Concurrency & Parallelism
Crucial for data loading and serving models.

| Model | Library | Use Case | GIL Impact |
| :--- | :--- | :--- | :--- |
| **Multi-threading** | `threading` | I/O-bound (Network, Disk) | **Blocked**. Only one thread runs Python bytecode at a time. |
| **Multi-processing** | `multiprocessing` | CPU-bound (Preprocessing) | **Bypassed**. Each process has its own Python interpreter and memory space. |
| **Asynchronous I/O** | `asyncio` | High-concurrency I/O (Web servers) | **Single-threaded**. Cooperative multitasking. No GIL issues (single thread). |

*   **The GIL (Global Interpreter Lock)**: A mutex that prevents multiple native threads from executing Python bytecodes simultaneously.
    *   *Why?* To protect reference counters in CPython's memory management.
    *   *Workaround*: Use `multiprocessing` or libraries that release the GIL (NumPy, PyTorch operations in C++).

### 2. Memory Management
*   **Reference Counting**: The primary mechanism. When an object's ref count hits 0, it's deallocated immediately.
*   **Garbage Collection (GC)**: A generational GC handles **reference cycles** (e.g., A refers to B, B refers to A). It runs periodically to find and clean up these isolated cycles.
*   **Weak References (`weakref`)**: References that don't increase the reference count. Useful for caches to avoid keeping objects alive unnecessarily.

### 3. Metaclasses
*   **Definition**: Classes are instances of metaclasses (default is `type`).
*   **Usage**: Intercepting class creation. Used in ORMs (Django, SQLAlchemy) and API frameworks (Pydantic, FastAPI) to validate class attributes or auto-generate code based on type hints.

### 4. Modern Python Ecosystem
AI Engineering requires robust, typed code.
*   **Type Hinting**: 
    *   Use `typing.List`, `typing.Optional`, `typing.Callable` (or standard collections in Python 3.9+).
    *   **Pydantic**: Data validation using python type hints. The backbone of most AI agents/tools.
*   **Packaging**:
    *   **`uv`**: An extremely fast Python package installer and resolver, written in Rust. Replaces pip/poetry for many.
    *   **`poetry`**: Dependency management with lockfiles (`poetry.lock`).
*   **Linting/Formatting**:
    *   **`ruff`**: An extremely fast Python linter and formatter, written in Rust. Replaces flake8, black, isort.

## Interview Questions

### Mid Level
1.  **Explain the difference between `is` and `==`.**
    *   *Answer*: `is` checks identity (memory address), `==` checks equality (value). `a = [1]; b = [1]`; `a == b` is True, `a is b` is False.
2.  **What are `*args` and `**kwargs`?**
    *   *Answer*: Variable length arguments. `*args` collects positional arguments into a tuple. `**kwargs` collects keyword arguments into a dictionary.
3.  **How do you copy an object in Python?**
    *   *Answer*: Assignment (`=`) creates a reference. `copy.copy()` creates a shallow copy (new container, references to same child objects). `copy.deepcopy()` creates a deep copy (recursively copies everything).
4.  **What is a lambda function?**
    *   *Answer*: An anonymous single-expression function. `lambda x: x * 2`.
5.  **Explain list comprehensions vs generator expressions.**
    *   *Answer*: List comps `[x*2 for x in range(10)]` create the full list in memory immediately. Generator expressions `(x*2 for x in range(10))` return a generator object and yield items one by one (lazy).

### Senior Level
1.  **How does the GIL impact training a Deep Learning model?**
    *   *Answer*: The training loop (forward/backward pass) usually runs in C++/CUDA (PyTorch/TF) and releases the GIL, so it's efficient. However, data preprocessing in Python (e.g., image transformations) can be a bottleneck. We use `DataLoader` with `num_workers > 0` (multiprocessing) to parallelize data loading and bypass the GIL.
2.  **Explain the difference between `__new__` and `__init__`.**
    *   *Answer*: `__new__` is a static method that *creates* and returns the instance. `__init__` initializes the already-created instance. `__new__` is used when subclassing immutable types (str, int, tuple) or implementing Singletons.
3.  **What is the difference between `@staticmethod` and `@classmethod`?**
    *   *Answer*: `staticmethod` is a plain function inside a class namespace (no `self` or `cls`). `classmethod` takes the class (`cls`) as the first argument. It's used for factory methods (alternative constructors).
4.  **How does Python handle memory management?**
    *   *Answer*: Primarily via reference counting. When ref count is 0, object is freed. A cyclic garbage collector (generational) runs periodically to clean up reference cycles that ref counting misses.
5.  **What are Context Managers?**
    *   *Answer*: Objects that define `__enter__` and `__exit__`. Used with the `with` statement to ensure resources (files, locks) are acquired and released deterministically, even if exceptions occur.

### Principal Level
1.  **Design a Python-based task scheduler.**
    *   *Discussion*: Discuss `asyncio` for the event loop to handle many waiting tasks. Use `heapq` (min-heap) for a priority queue of scheduled tasks. Use `multiprocessing` or `ProcessPoolExecutor` for CPU-intensive tasks to avoid blocking the event loop. Handle graceful shutdowns and signal handling (`SIGINT`).
2.  **How would you optimize a Python application that is suffering from high memory fragmentation?**
    *   *Discussion*:
        *   Use `__slots__` to reduce per-object overhead.
        *   Use `array` module or NumPy for homogeneous data instead of lists.
        *   Switch to a custom memory allocator like `jemalloc` or `tcmalloc` (via `LD_PRELOAD`) which handles fragmentation better than `malloc`.
        *   Force GC collection `gc.collect()` at strategic times (though usually not recommended).
3.  **Compare `multiprocessing` vs `asyncio` for a microservice.**
    *   *Discussion*:
        *   **I/O Bound (DB queries, external APIs)**: `asyncio` is superior. Lower overhead (single thread), handles thousands of concurrent connections.
        *   **CPU Bound (Image processing, ML inference)**: `multiprocessing` is required to utilize multiple cores.
        *   **Hybrid**: Use `asyncio` for the server (FastAPI) and offload CPU tasks to a `ProcessPoolExecutor`.
4.  **Explain Method Resolution Order (MRO) and the Diamond Problem.**
    *   *Discussion*: Python uses C3 Linearization. It ensures that a class always appears before its parents and parents appear in the order listed. It prevents the issue where a base class is initialized multiple times or methods are resolved ambiguously in complex multiple inheritance graphs.
