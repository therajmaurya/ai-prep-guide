---
title: "NumPy"
category: coding
levels: ["mid", "senior", "principal"]
skills: [python, numpy, linear-algebra, performance-optimization, scientific-computing]
questions:
  "mid": ["What is broadcasting?", "Difference between view and copy.", "How to filter an array using a boolean mask?"]
  "senior": ["Explain memory layout (strides) in NumPy.", "Optimize a slow loop using vectorization.", "Difference between `np.dot` and `np.matmul`."]
  "principal": ["Implement a custom ufunc in C/Cython.", "Discuss BLAS/LAPACK integration in NumPy.", "How to handle arrays larger than memory?"]
---

# NumPy for Data Science Interviews

NumPy (Numerical Python) is the foundational library for scientific computing in Python. It provides support for large, multi-dimensional arrays and matrices, along with a collection of mathematical functions to operate on these arrays efficiently.

## Core Concepts

### 1. The `ndarray` Object
*   **Homogeneous**: All elements must be of the same type (unlike Python lists).
*   **Contiguous Memory**: Stored in a contiguous block of memory, allowing for efficient cache utilization and SIMD operations.
*   **Attributes**:
    *   `ndim`: Number of dimensions.
    *   `shape`: Tuple of array dimensions.
    *   `size`: Total number of elements.
    *   `dtype`: Data type (e.g., `float32`, `int64`).
    *   `strides`: Tuple of bytes to step in each dimension when traversing an array.

### 2. Array Creation
Beyond `np.array()`, know the efficient initializers:
*   `np.zeros()`, `np.ones()`: Initialize with 0s or 1s.
*   `np.empty()`: Uninitialized (garbage values). Fastest if you plan to fill it immediately.
*   `np.arange()`: Like Python's `range` but returns array.
*   `np.linspace()`: Linearly spaced values (e.g., 10 points between 0 and 1).

### 3. Broadcasting
Broadcasting allows NumPy to treat arrays with different shapes during arithmetic operations.

**Rules**:
1.  If arrays differ in rank, prepend 1s to the smaller shape.
2.  Arrays are compatible if dimensions match or one of them is 1.
3.  The array with size 1 is stretched to match the other.

**Example**:
```python
A = np.array([[1, 2, 3], [4, 5, 6]]) # Shape (2, 3)
b = np.array([10, 20, 30])           # Shape (3,) -> (1, 3) via Rule 1
# Result: b is broadcast across rows of A
```

### 4. Indexing & Slicing
*   **Basic Slicing**: `arr[start:stop:step]`. Returns a **view** (no copy). Modifying the view modifies the original.
*   **Advanced Indexing**: Returns a **copy**.
    *   **Boolean Masking**: `arr[arr > 5]`.
    *   **Fancy Indexing**: `arr[[1, 3, 5]]` (Using integer arrays).
*   **`np.where`**: `np.where(condition, x, y)`. Vectorized "if-else".

## Advanced Topics

### 1. Memory Layout & Strides
*   **Row-Major (C-order)**: Default. Last index changes fastest. `(0,0) -> (0,1) -> (0,2)`.
*   **Column-Major (Fortran-order)**: First index changes fastest. `(0,0) -> (1,0) -> (2,0)`.
*   **Impact**: Traversing an array in the wrong order (against the grain of memory) causes cache misses.
    *   Iterating over rows in C-order is fast.
    *   Iterating over columns in C-order is slow.

### 2. Linear Algebra (`np.linalg`)
*   **Dot Product**: `np.dot(a, b)` (matrix multiplication for 2D).
*   **Matmul**: `a @ b` or `np.matmul(a, b)`. Preferred for 3D+ arrays (treats them as stacks of matrices).
*   **Decompositions**: `np.linalg.svd`, `np.linalg.eig`, `np.linalg.cholesky`.
*   **Solving Systems**: `np.linalg.solve(A, b)` is numerically more stable than `inv(A) @ b`.

### 3. Performance Optimization
*   **Vectorization**: Replacing explicit loops with array expressions. Pushes loops to C level.
*   **Universal Functions (ufuncs)**: Element-wise operations (add, sin, exp) that support broadcasting.
*   **Numba**: JIT compiler for Python. Decorate functions with `@jit(nopython=True)` to compile them to machine code. Works best with NumPy loops.
    ```python
    from numba import jit

    @jit(nopython=True)
    def fast_loop(arr):
        res = 0
        for x in arr:
            res += x
        return res
    ```

## Interview Questions

### Mid Level
1.  **What is the difference between a Python list and a NumPy array?**
    *   *Answer*: Lists are general-purpose containers (pointers to objects), slow, and memory-heavy. Arrays are fixed-type, contiguous memory, fast (vectorized), and memory-efficient.
2.  **Explain Broadcasting with an example.**
    *   *Answer*: Adding a scalar to a matrix (`mat + 5`) or adding a row vector to a matrix. NumPy stretches the smaller array virtually without copying data.
3.  **What is the difference between `np.copy()` and `np.view()`?**
    *   *Answer*: `view` creates a new array object looking at the *same data*. Modifying it affects the original. `copy` creates a deep copy of the data.
4.  **How do you concatenate two arrays?**
    *   *Answer*: `np.concatenate([a, b], axis=0)` or `np.vstack` / `np.hstack`.

### Senior Level
1.  **Why is `arr[mask]` slower than `arr[slice]`?**
    *   *Answer*: Boolean masking (advanced indexing) creates a *copy* of the data because the result might not be contiguous. Slicing returns a *view* which is just a metadata change (pointer arithmetic).
2.  **How would you optimize a function that calculates the pairwise Euclidean distance between two large sets of vectors?**
    *   *Answer*: Use broadcasting: `sqrt(sum((A[:, None] - B[None, :])**2, axis=-1))`. This avoids explicit loops and uses optimized C code. Alternatively, use `scipy.spatial.distance.cdist`.
3.  **Explain the concept of "Strides" in NumPy.**
    *   *Answer*: Strides are the number of bytes to jump to get to the next element in each dimension. They allow for zero-copy slicing (e.g., `arr[::-1]` just changes the stride to negative).
4.  **What is the difference between `np.nan` and `None`?**
    *   *Answer*: `np.nan` is a floating-point value (IEEE 754). It allows vectorized operations (NaN propagates). `None` is a Python object, forcing the array to be of object dtype (slow).

### Principal Level
1.  **How does NumPy handle memory mapping for datasets larger than RAM?**
    *   *Discussion*: `np.memmap` allows accessing a file on disk as if it were an array in memory, using the OS paging system to load chunks as needed.
2.  **Discuss the Global Interpreter Lock (GIL) in the context of NumPy.**
    *   *Discussion*: Many NumPy operations (like dot products, SVD) release the GIL, allowing multi-threaded execution on multi-core CPUs, unlike pure Python code. This is often done via calls to underlying BLAS/LAPACK libraries (like MKL or OpenBLAS).
3.  **Write a custom ufunc to optimize a specific mathematical operation.**
    *   *Discussion*: Using `numba.vectorize` or writing a C extension (using NumPy C API) to create a ufunc that runs at C-speed and supports broadcasting.
4.  **How would you implement a rolling window average efficiently in NumPy without loops?**
    *   *Discussion*: Use `np.lib.stride_tricks.as_strided` to create a view of the array with a new shape (windows) without copying data, then take the mean. Or use `np.convolve`.
