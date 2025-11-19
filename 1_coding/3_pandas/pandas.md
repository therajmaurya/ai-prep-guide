---
title: "Pandas"
category: coding
levels: ["mid", "senior", "principal"]
skills: [python, pandas, data-manipulation, performance-optimization]
questions:
  "mid": ["Explain the difference between loc and iloc.", "How do you handle missing values?"]
  "senior": ["How does GroupBy split-apply-combine work internally?", "Optimize a slow Pandas operation."]
  "principal": ["Design a data processing pipeline for terabyte-scale data using Pandas/Dask.", "Discuss memory management in Pandas."]
---

# Pandas for Data Science Interviews

Pandas is the workhorse of data manipulation in Python. Mastering it goes beyond just knowing the API; it involves understanding memory usage, vectorization, and efficient data processing patterns.

## Core Concepts

### 1. Data Structures
- **Series**: 1D labeled array. Homogeneous data types (mostly).
- **DataFrame**: 2D labeled data structure. Heterogeneous columns.
- **Index**: Immutable sequence used for labeling/indexing.

### 2. Indexing & Selection
- **`.loc[]`**: Label-based. Inclusive of start and stop.
- **`.iloc[]`**: Integer-position based. Exclusive of stop (like Python lists).
- **Boolean Indexing**: `df[df['col'] > 5]` - Uses a boolean mask.
- **`query()`**: `df.query('col > 5')` - Can be faster and more readable.

### 3. GroupBy: Split-Apply-Combine
- **Split**: Data is divided into groups based on keys.
- **Apply**: A function (aggregation, transformation, filtration) is applied to each group.
- **Combine**: Results are combined into a new object.
- *Tip*: Use `transform()` to return an object with the same index as the original (useful for feature engineering like "mean encoding").

### 4. Merging & Joining
- **`merge()`**: Database-style joins (inner, outer, left, right) on columns.
- **`join()`**: Join on index (by default).
- **`concat()`**: Stack DataFrames vertically (axis=0) or horizontally (axis=1).

## Advanced Topics

### 1. Performance Optimization
- **Vectorization**: Avoid loops! Use built-in Pandas/NumPy functions which are implemented in C.
- **`apply()` vs Vectorized**: `apply()` is often just a loop in Python. Use it only when necessary.
- **Cython/Numba**: For custom functions that can't be vectorized.
- **Categorical Data**: Convert string columns with low cardinality to `category` dtype to save memory and speed up GroupBy.
  ```python
  df['grade'] = df['grade'].astype('category')
  ```

### 2. Handling Large Datasets
- **Chunking**: Read data in chunks using `chunksize` in `read_csv`.
- **Dtypes**: Downcast numeric types (e.g., `float64` -> `float32`, `int64` -> `int16`).
- **Parquet/Feather**: Use binary formats instead of CSV for faster I/O.

### 3. Time Series
- **`resample()`**: Frequency conversion (e.g., daily to monthly).
- **`rolling()`**: Moving window calculations.
- **`shift()`/`diff()`**: Lag features and differencing.

## Interview Questions

### Mid Level
1.  **What is the difference between `loc` and `iloc`?**
    *   *Answer*: `loc` is label-based (uses index names), `iloc` is integer-position based (0 to length-1).
2.  **How do you handle missing values in a DataFrame?**
    *   *Answer*: `dropna()` to remove, `fillna()` to replace (mean, median, constant), or interpolation.
3.  **Explain the difference between `merge` and `concat`.**
    *   *Answer*: `merge` joins on columns (SQL-style join), `concat` appends dataframes along an axis (stacking).

### Senior Level
1.  **Why is looping over a DataFrame slow? How can you fix it?**
    *   *Answer*: Python loops have overhead for type checking and pointer dereferencing. Pandas/NumPy operations are vectorized in C, operating on contiguous memory blocks. Fix: Use vectorized operations or `apply()` (as a last resort).
2.  **What is the "SettingWithCopyWarning" and how do you avoid it?**
    *   *Answer*: It happens when you modify a slice of a DataFrame that might be a view or a copy. Avoid chained indexing (`df[mask]['col'] = 5`). Use `.loc[mask, 'col'] = 5` instead.
3.  **How does `groupby().transform()` differ from `groupby().agg()`?**
    *   *Answer*: `agg` returns a reduced result (one row per group), `transform` returns a result with the same shape as the original input (broadcasts the result back to the original index).

### Principal Level
1.  **How would you process a 100GB CSV file on a machine with 16GB RAM?**
    *   *Discussion*: Use `chunksize` to process in batches, use `dask` or `polars` for out-of-core processing, or load only necessary columns with `usecols` and optimized dtypes.
2.  **Explain the memory layout of a DataFrame. Why are mixed types slower?**
    *   *Discussion*: Pandas is built on NumPy. A DataFrame is a collection of Series. If columns have different types, they are stored in separate memory blocks. Operations across mixed types prevent efficient vectorization and cache utilization.
3.  **Design a feature engineering pipeline that is robust to data leakage.**
    *   *Discussion*: Ensure all transformations (scaling, encoding, imputation) are fit *only* on the training set and then applied to validation/test sets. Use `sklearn.pipeline.Pipeline`.
