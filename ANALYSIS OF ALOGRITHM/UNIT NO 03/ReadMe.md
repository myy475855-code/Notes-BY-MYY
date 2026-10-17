# Searching and Sorting Algorithms — Complete Notes

**Subject:** Data Structures and Algorithms (DSA)  
**Topics:** Linear Search, Binary Search, Bubble Sort, Selection Sort, Insertion Sort, Bucket Sort, Radix Sort  
**Purpose:** Study notes, exam preparation, and step-by-step tracing practice

---

## Table of Contents

1. [Introduction to Searching and Sorting](#1-introduction-to-searching-and-sorting)
2. [Algorithm Analysis](#2-algorithm-analysis)
3. [Linear Search](#3-linear-search)
4. [Binary Search](#4-binary-search)
5. [Bubble Sort](#5-bubble-sort)
6. [Selection Sort](#6-selection-sort)
7. [Insertion Sort](#7-insertion-sort)
8. [Bucket Sort](#8-bucket-sort)
9. [Radix Sort](#9-radix-sort)
10. [Algorithm Comparison Tables](#10-algorithm-comparison-tables)
11. [Important Exam Questions](#11-important-exam-questions)
12. [Quick Revision Sheet](#12-quick-revision-sheet)

---

# 1. Introduction to Searching and Sorting

## 1.1 What Is Searching?

**Searching** is the process of finding a particular element in a collection of data.

The element we want to find is called the **target** or **key**.

**Example**

Array: `10, 25, 30, 45, 60`  
Target: `45`

Result: The target is found at index `3` (the fourth position), because array indexing starts from zero.

Common searching algorithms:

- Linear Search
- Binary Search

## 1.2 What Is Sorting?

**Sorting** is the process of arranging data in a specific order, usually ascending or descending.

**Example**

Unsorted array:

`40, 10, 30, 20, 50`

Ascending order:

`10, 20, 30, 40, 50`

Descending order:

`50, 40, 30, 20, 10`

Common sorting algorithms:

- Bubble Sort
- Selection Sort
- Insertion Sort
- Bucket Sort
- Radix Sort

## 1.3 Difference Between Searching and Sorting

| Searching | Sorting |
|---|---|
| Finds a particular element. | Arranges elements in order. |
| Output is usually a position or found/not found. | Output is an ordered collection. |
| Example: find `30`. | Example: arrange `30, 10, 20`. |
| Linear Search and Binary Search. | Bubble, Selection, Insertion, Bucket, and Radix Sort. |

---

# 2. Algorithm Analysis

Algorithm analysis measures how efficiently an algorithm uses time and memory as input size increases.

## 2.1 Time Complexity

**Time complexity** describes how the number of operations grows with input size `n`. It does not simply measure actual time in seconds.

For example, if an algorithm checks every element in an array of 100 elements, it may perform up to 100 checks.

## 2.2 Space Complexity

**Space complexity** describes the memory an algorithm requires as input size grows. In these notes, auxiliary space means the extra memory used by the algorithm, excluding the input array itself.

For example, an in-place sorting algorithm may rearrange an array without creating another array proportional to its size.

## 2.3 Big-O Notation

Big-O notation describes an asymptotic upper bound on an algorithm's growth rate. It is commonly used to express worst-case complexity.

| Complexity | Name | General meaning |
|---|---|---|
| `O(1)` | Constant | Work stays approximately constant as input grows. |
| `O(log n)` | Logarithmic | The problem size is repeatedly reduced, often by half. |
| `O(n)` | Linear | Work can grow proportionally to the number of elements. |
| `O(n log n)` | Linearithmic | Common in efficient comparison sorting algorithms. |
| `O(n²)` | Quadratic | Work can grow proportionally to the square of input size. |
| `O(2ⁿ)` | Exponential | Work becomes expensive very quickly as input grows. |

These growth rates help compare algorithms as input sizes become large. Actual performance also depends on implementation, input distribution, and hardware.

## 2.4 Best, Average, and Worst Cases

- **Best case:** The minimum work needed for an input of size `n`.
- **Average case:** The expected work over an assumed distribution of inputs.
- **Worst case:** The maximum work for an input of size `n`.

For example, Linear Search can find the target on the first comparison, giving a best case of `O(1)`. If the target is at the end or absent, it may need `n` comparisons, giving a worst case of `O(n)`.

---

# 3. Linear Search

## 3.1 Definition

**Linear Search** is a searching algorithm that checks each element one by one from the beginning until the target is found or all elements have been checked.

It is also called **Sequential Search**.

## 3.2 Working Principle

1. Start from the first element.
2. Compare the current element with the target.
3. If they are equal, return the index.
4. Otherwise, move to the next element.
5. If all elements are checked and no match is found, return `-1` or “Not Found.”

Linear Search works on both sorted and unsorted arrays.

## 3.3 Step-by-Step Example

Given array:

`A = [12, 25, 7, 34, 18, 40]`

Target: `34`

Array indices:

| Index | 0 | 1 | 2 | 3 | 4 | 5 |
|---|---:|---:|---:|---:|---:|---:|
| Value | 12 | 25 | 7 | 34 | 18 | 40 |

Search trace:

| Step | Current value | Comparison | Result |
|---|---:|---|---|
| 1 | 12 | `12 == 34`? | No |
| 2 | 25 | `25 == 34`? | No |
| 3 | 7 | `7 == 34`? | No |
| 4 | 34 | `34 == 34`? | Yes — found |

**Result:** Target found at index `3`.  
**Total comparisons:** `4`.

## 3.4 Pseudocode

```text
LinearSearch(A, n, key)
    for i = 0 to n - 1
        if A[i] == key
            return i
    return -1
```

Here, `A` is the array, `n` is the number of elements, and `key` is the target.

## 3.5 Complexity Analysis

| Case | Time complexity | Reason |
|---|---|---|
| Best | `O(1)` | Target is the first element. |
| Average | `O(n)` | A linear number of elements is checked under common assumptions. |
| Worst | `O(n)` | Target is last or absent. |
| Auxiliary space | `O(1)` | Only a few variables are needed. |

## 3.6 Advantages

- Simple to understand and implement.
- Works with sorted and unsorted data.
- Does not require extra memory proportional to array size.
- Suitable for small lists.

## 3.7 Disadvantages

- Can be slow for large lists.
- May require checking every element.
- Usually less efficient than Binary Search on sorted arrays.

## 3.8 Applications

- Searching a small list of names.
- Checking a short array.
- Finding an item in unsorted data.
- Searching data when sorting it first would be unnecessary overhead.

---

# 4. Binary Search

## 4.1 Definition

**Binary Search** is a searching algorithm that repeatedly divides the search interval into two halves until the target is found or the interval becomes empty.

**Important condition:** The array must be sorted for ordinary Binary Search to work correctly.

## 4.2 Working Principle

1. Set `low` to the first index and `high` to the last index.
2. Calculate the middle index:

   `mid = floor((low + high) / 2)`

3. Compare the middle element with the target.
4. If equal, return the middle index.
5. If the target is smaller, search the left half.
6. If the target is larger, search the right half.
7. Repeat until the target is found or `low > high`.

## 4.3 Step-by-Step Example

Given sorted array:

`A = [10, 20, 30, 40, 50, 60, 70, 80, 90]`

Target: `70`

### Step 1: Find the Middle

`low = 0`, `high = 8`

`mid = floor((0 + 8) / 2) = 4`

`A[4] = 50`

Since `70 > 50`, discard the left half, including index `4`.

New range: `low = 5`, `high = 8`.

### Step 2: Search the Right Half

Current range: `[60, 70, 80, 90]`

`mid = floor((5 + 8) / 2) = 6`

`A[6] = 70`

The target is found at index `6`.

| Step | Low | High | Mid | Middle value | Decision |
|---|---:|---:|---:|---:|---|
| 1 | 0 | 8 | 4 | 50 | Target is larger; search right. |
| 2 | 5 | 8 | 6 | 70 | Found. |

**Result:** Target found at index `6`.  
**Total comparisons:** `2`.

## 4.4 Another Way to Understand Binary Search

Imagine searching for a word in a dictionary. Instead of starting at page one and inspecting every page, you open near the middle and decide which half to search. Binary Search uses the same idea.

## 4.5 Pseudocode

```text
BinarySearch(A, n, key)
    low = 0
    high = n - 1

    while low <= high
        mid = low + floor((high - low) / 2)

        if A[mid] == key
            return mid
        else if key < A[mid]
            high = mid - 1
        else
            low = mid + 1

    return -1
```

The expression `low + floor((high - low) / 2)` avoids the potential overflow that can occur in some languages with `(low + high) / 2`.

## 4.6 Complexity Analysis

| Case | Time complexity | Reason |
|---|---|---|
| Best | `O(1)` | Target is the first middle element checked. |
| Average | `O(log n)` | The search range repeatedly halves. |
| Worst | `O(log n)` | Repeated halving until found or exhausted. |
| Auxiliary space, iterative | `O(1)` | Uses a fixed number of variables. |
| Auxiliary space, recursive | `O(log n)` | Recursive calls use stack space. |

### Why Is Binary Search `O(log n)`?

Suppose there are 16 elements:

`16 → 8 → 4 → 2 → 1`

About four halvings reduce the range from 16 elements to one. The number of halvings grows approximately as `log₂(n)`.

## 4.7 Advantages

- Very efficient for large, sorted arrays.
- Requires far fewer comparisons than Linear Search in many cases.
- Iterative implementation uses constant auxiliary space.

## 4.8 Disadvantages

- Requires sorted data.
- Sorting data first may have a cost.
- It is not generally efficient on linked lists because finding the middle element is not constant-time.

## 4.9 Linear Search vs Binary Search

| Feature | Linear Search | Binary Search |
|---|---|---|
| Data requirement | Sorted or unsorted | Sorted |
| Best time | `O(1)` | `O(1)` |
| Average time | `O(n)` | `O(log n)` |
| Worst time | `O(n)` | `O(log n)` |
| Iterative auxiliary space | `O(1)` | `O(1)` |
| Main idea | Check one by one | Repeatedly halve the range |

---

# 5. Bubble Sort

## 5.1 Definition

**Bubble Sort** is a comparison-based sorting algorithm that repeatedly compares adjacent elements and swaps them when they are in the wrong order.

After each complete pass in ascending order, the largest unsorted element moves to the end of the unsorted portion.

## 5.2 Working Principle

1. Start at the first pair of adjacent elements.
2. Compare the pair.
3. Swap them if the left element is greater than the right element.
4. Continue to the end of the unsorted portion.
5. Repeat passes until the array is sorted.
6. An optimized version stops early if a complete pass makes no swaps.

## 5.3 Step-by-Step Example

Sort in ascending order:

`[5, 1, 4, 2, 8]`

### Pass 1

| Comparison | Action | Array after action |
|---|---|---|
| 5 and 1 | Swap | `[1, 5, 4, 2, 8]` |
| 5 and 4 | Swap | `[1, 4, 5, 2, 8]` |
| 5 and 2 | Swap | `[1, 4, 2, 5, 8]` |
| 5 and 8 | No swap | `[1, 4, 2, 5, 8]` |

The largest element, `8`, is at the end after this pass.

### Pass 2

Start with `[1, 4, 2, 5, 8]`.

| Comparison | Action | Array after action |
|---|---|---|
| 1 and 4 | No swap | `[1, 4, 2, 5, 8]` |
| 4 and 2 | Swap | `[1, 2, 4, 5, 8]` |
| 4 and 5 | No swap | `[1, 2, 4, 5, 8]` |

### Pass 3

Start with `[1, 2, 4, 5, 8]`.

- Compare `1` and `2`: no swap.
- Compare `2` and `4`: no swap.

The optimized algorithm notices that no swaps occurred and stops.

**Final sorted array:** `[1, 2, 4, 5, 8]`

## 5.4 Pseudocode

```text
BubbleSort(A, n)
    for i = 0 to n - 2
        swapped = false

        for j = 0 to n - 2 - i
            if A[j] > A[j + 1]
                swap A[j], A[j + 1]
                swapped = true

        if swapped == false
            break
```

## 5.5 Complexity Analysis

| Case | Time complexity |
|---|---|
| Best, with early-stop flag | `O(n)` |
| Average | `O(n²)` |
| Worst | `O(n²)` |
| Auxiliary space | `O(1)` |

The maximum number of adjacent comparisons in the standard version is approximately:

`(n - 1) + (n - 2) + ... + 1 = n(n - 1) / 2`

This grows proportionally to `n²`.

## 5.6 Properties

- **Stable:** Yes, if equal elements are not swapped.
- **In-place:** Yes.
- **Adaptive:** Yes, with an early-stop optimization.
- **Suitable for:** Learning sorting and small datasets.
- **Main disadvantage:** Poor performance on large datasets.

## 5.7 Advantages and Disadvantages

**Advantages**
- Easy to understand and implement.
- Requires constant auxiliary space.
- Optimized version detects an already sorted array quickly.

**Disadvantages**
- Average and worst-case time are quadratic.
- Performs many comparisons and swaps compared with more efficient algorithms.

---

# 6. Selection Sort

## 6.1 Definition

**Selection Sort** repeatedly selects the smallest element from the unsorted portion and places it at the beginning of that portion.

It divides the array conceptually into two sections:

- Sorted portion on the left.
- Unsorted portion on the right.

## 6.2 Working Principle

1. Assume the first unsorted element is the minimum.
2. Scan the remaining unsorted elements.
3. Find the actual minimum element.
4. Swap it with the first unsorted element.
5. Move the boundary of the sorted portion one position to the right.
6. Repeat until the array is sorted.

## 6.3 Step-by-Step Example

Sort:

`[64, 25, 12, 22, 11]`

### Pass 1

Unsorted array: `[64, 25, 12, 22, 11]`

- Minimum initially = `64`.
- Compare with `25`: minimum becomes `25`.
- Compare with `12`: minimum becomes `12`.
- Compare with `22`: minimum remains `12`.
- Compare with `11`: minimum becomes `11`.
- Swap `64` with `11`.

Result: `[11, 25, 12, 22, 64]`

### Pass 2

Unsorted portion: `[25, 12, 22, 64]`

- Minimum initially = `25`.
- Compare with `12`: minimum becomes `12`.
- Compare with `22` and `64`: minimum remains `12`.
- Swap `25` with `12`.

Result: `[11, 12, 25, 22, 64]`

### Pass 3

Unsorted portion: `[25, 22, 64]`

- Minimum initially = `25`.
- Compare with `22`: minimum becomes `22`.
- Compare with `64`: minimum remains `22`.
- Swap `25` with `22`.

Result: `[11, 12, 22, 25, 64]`

### Pass 4

Unsorted portion: `[25, 64]`

- Compare `25` and `64`.
- Minimum is `25`, so no swap is necessary.

**Final sorted array:** `[11, 12, 22, 25, 64]`

## 6.4 Pass Summary

| Pass | Array after the pass | Sorted prefix |
|---|---|---|
| 1 | `[11, 25, 12, 22, 64]` | `[11]` |
| 2 | `[11, 12, 25, 22, 64]` | `[11, 12]` |
| 3 | `[11, 12, 22, 25, 64]` | `[11, 12, 22]` |
| 4 | `[11, 12, 22, 25, 64]` | `[11, 12, 22, 25]` |

## 6.5 Pseudocode

```text
SelectionSort(A, n)
    for i = 0 to n - 2
        minIndex = i

        for j = i + 1 to n - 1
            if A[j] < A[minIndex]
                minIndex = j

        if minIndex != i
            swap A[i], A[minIndex]
```

## 6.6 Complexity Analysis

| Case | Time complexity |
|---|---|
| Best | `O(n²)` |
| Average | `O(n²)` |
| Worst | `O(n²)` |
| Auxiliary space | `O(1)` |

Selection Sort performs approximately `n(n - 1) / 2` comparisons regardless of whether the array is already sorted.

## 6.7 Properties

- **Stable:** Usually no, in its standard swap-based implementation.
- **In-place:** Yes.
- **Adaptive:** No; standard Selection Sort still scans the unsorted portion.
- **Number of swaps:** At most `n - 1` swaps.
- **Best use:** Small arrays or situations where minimizing writes is important.

## 6.8 Advantages and Disadvantages

**Advantages**
- Simple to implement.
- Uses constant auxiliary space.
- Makes relatively few swaps.

**Disadvantages**
- Always performs quadratic comparisons.
- Usually unstable in its standard form.
- Not suitable for large datasets.

---

# 7. Insertion Sort

## 7.1 Definition

**Insertion Sort** builds a sorted portion of the array one element at a time. Each new element is inserted into its correct position among the elements already sorted.

It works similarly to arranging playing cards in your hand.

## 7.2 Working Principle

1. Consider the first element sorted.
2. Select the next element as the `key`.
3. Compare the key with elements on its left.
4. Shift elements greater than the key one position to the right.
5. Insert the key in the empty position.
6. Repeat for each remaining element.

## 7.3 Step-by-Step Example

Sort:

`[5, 2, 4, 6, 1, 3]`

### Initial Array

`[5, 2, 4, 6, 1, 3]`

The first element, `5`, is initially treated as sorted.

### Pass 1: Insert `2`

- Key = `2`.
- Compare `2` with `5`.
- Since `5 > 2`, shift `5` right.
- Insert `2` at the beginning.

Result: `[2, 5, 4, 6, 1, 3]`

### Pass 2: Insert `4`

- Key = `4`.
- Compare `4` with `5`: shift `5` right.
- Compare `4` with `2`: stop because `2 < 4`.
- Insert `4` after `2`.

Result: `[2, 4, 5, 6, 1, 3]`

### Pass 3: Insert `6`

- Key = `6`.
- Compare `6` with `5`.
- Since `5 < 6`, no shifting is needed.

Result: `[2, 4, 5, 6, 1, 3]`

### Pass 4: Insert `1`

- Key = `1`.
- Shift `6` right.
- Shift `5` right.
- Shift `4` right.
- Shift `2` right.
- Insert `1` at the beginning.

Result: `[1, 2, 4, 5, 6, 3]`

### Pass 5: Insert `3`

- Key = `3`.
- Shift `6` right.
- Shift `5` right.
- Shift `4` right.
- Stop at `2`.
- Insert `3` after `2`.

Result: `[1, 2, 3, 4, 5, 6]`

**Final sorted array:** `[1, 2, 3, 4, 5, 6]`

## 7.4 Pass Summary

| Pass | Key | Array after insertion |
|---|---:|---|
| 1 | 2 | `[2, 5, 4, 6, 1, 3]` |
| 2 | 4 | `[2, 4, 5, 6, 1, 3]` |
| 3 | 6 | `[2, 4, 5, 6, 1, 3]` |
| 4 | 1 | `[1, 2, 4, 5, 6, 3]` |
| 5 | 3 | `[1, 2, 3, 4, 5, 6]` |

## 7.5 Pseudocode

```text
InsertionSort(A, n)
    for i = 1 to n - 1
        key = A[i]
        j = i - 1

        while j >= 0 and A[j] > key
            A[j + 1] = A[j]
            j = j - 1

        A[j + 1] = key
```

## 7.6 Complexity Analysis

| Case | Time complexity |
|---|---|
| Best | `O(n)` |
| Average | `O(n²)` |
| Worst | `O(n²)` |
| Auxiliary space | `O(1)` |

If the array is already sorted, each key requires only one main comparison and no shifting, so the best-case time is `O(n)`.

## 7.7 Properties

- **Stable:** Yes, if equal elements are not shifted past one another.
- **In-place:** Yes.
- **Adaptive:** Yes.
- **Best use:** Small arrays and arrays that are already nearly sorted.
- **Main disadvantage:** Slow on large, randomly ordered arrays.

## 7.8 Advantages and Disadvantages

**Advantages**
- Simple to understand and implement.
- Efficient for small or nearly sorted arrays.
- Stable and in-place.
- Can process data as it arrives.

**Disadvantages**
- Average and worst-case time are quadratic.
- Requires many shifts when elements are in reverse order.

---

# 8. Bucket Sort

## 8.1 Definition

**Bucket Sort** is a sorting algorithm that distributes elements into several groups called **buckets**, sorts the elements within each bucket, and then combines the buckets in order.

It is particularly effective when values are distributed fairly evenly across a known range.

## 8.2 Working Principle

1. Create several empty buckets.
2. Assign each element to an appropriate bucket based on its value.
3. Sort each bucket individually.
4. Concatenate the buckets from lowest to highest.

## 8.3 Step-by-Step Example

Sort these decimal values in ascending order:

`[0.42, 0.32, 0.23, 0.52, 0.25, 0.47, 0.51]`

Assume values are between `0` (inclusive) and `1` (exclusive), and use five buckets:

- Bucket 0: values from `0.0` to below `0.2`
- Bucket 1: values from `0.2` to below `0.4`
- Bucket 2: values from `0.4` to below `0.6`
- Bucket 3: values from `0.6` to below `0.8`
- Bucket 4: values from `0.8` to below `1.0`

For this example, assign a value `x` using:

`bucket_index = floor(5 * x)`

All example values are less than `1`, so the bucket index is between `0` and `4`.

### Step 1: Distribute the Values

| Value | Calculation | Bucket |
|---|---|---:|
| 0.42 | `floor(5 × 0.42) = 2` | 2 |
| 0.32 | `floor(5 × 0.32) = 1` | 1 |
| 0.23 | `floor(5 × 0.23) = 1` | 1 |
| 0.52 | `floor(5 × 0.52) = 2` | 2 |
| 0.25 | `floor(5 × 0.25) = 1` | 1 |
| 0.47 | `floor(5 × 0.47) = 2` | 2 |
| 0.51 | `floor(5 × 0.51) = 2` | 2 |

### Step 2: Examine the Buckets

- **Bucket 0:** Empty
- **Bucket 1:** `[0.32, 0.23, 0.25]`
- **Bucket 2:** `[0.42, 0.52, 0.47, 0.51]`
- **Bucket 3:** Empty
- **Bucket 4:** Empty

### Step 3: Sort Each Bucket

Using Insertion Sort inside each bucket:

- Bucket 1 becomes `[0.23, 0.25, 0.32]`.
- Bucket 2 becomes `[0.42, 0.47, 0.51, 0.52]`.
- The other buckets remain empty.

### Step 4: Concatenate the Buckets

Read the buckets from 0 to 4:

`[0.23, 0.25, 0.32, 0.42, 0.47, 0.51, 0.52]`

**Final sorted array:** `[0.23, 0.25, 0.32, 0.42, 0.47, 0.51, 0.52]`

## 8.4 Pseudocode

```text
BucketSort(A, n)
    create k empty buckets

    for each value x in A
        b = bucket_index(x)
        insert x into bucket[b]

    for each bucket
        sort the bucket

    combine buckets in order
```

The `bucket_index` calculation depends on the input range and how the buckets are defined.

## 8.5 Complexity Analysis

Let:
- `n` = number of elements.
- `k` = number of buckets.

| Case | Time complexity |
|---|---|
| Best / expected under suitable distribution | `O(n + k)` |
| Average, under common uniform-distribution assumptions | `O(n + k)` |
| Worst, with a simple comparison sort inside one heavily populated bucket | `O(n²)` |
| Auxiliary space | `O(n + k)` |

The exact complexity depends on the distribution of values, the number of buckets, and the algorithm used to sort each bucket.

## 8.6 Advantages

- Can be very fast for suitably distributed data.
- Values are processed in groups.
- Useful for numerical data within a known range.

## 8.7 Disadvantages

- Performance depends on bucket distribution.
- Requires additional memory.
- Poor bucket design can place nearly all values into one bucket.

## 8.8 Applications

- Sorting uniformly distributed decimal values.
- Numerical data grouped by range.
- Certain data-processing tasks where the value range is known.

---

# 9. Radix Sort

## 9.1 Definition

**Radix Sort** is a non-comparison-based sorting algorithm that sorts numbers or strings by processing their individual digits or characters.

A common version, **LSD Radix Sort**, processes digits from the **least significant digit** to the **most significant digit**.

For decimal numbers, the digit positions are:

- Units: `10⁰`
- Tens: `10¹`
- Hundreds: `10²`
- Thousands: `10³`

Radix Sort usually uses a **stable sorting method**, such as Counting Sort, at each digit position.

## 9.2 Working Principle

1. Find the maximum number to determine the number of digit positions needed.
2. Sort the numbers by their units digit.
3. Maintain the relative order of numbers with equal units digits.
4. Sort the resulting list by tens digit, again stably.
5. Continue with hundreds, thousands, and any remaining positions.
6. Stop after processing the most significant digit.

## 9.3 Step-by-Step Example

Sort:

`[170, 45, 75, 90, 802, 24, 2, 66]`

The maximum number is `802`, which has three digits. Therefore, three passes are needed.

For alignment, we can think of the numbers as:

`170, 045, 075, 090, 802, 024, 002, 066`

Leading zeros are only for explanation; they do not change the values.

### Pass 1: Sort by Units Digit

| Number | Units digit |
|---|---:|
| 170 | 0 |
| 45 | 5 |
| 75 | 5 |
| 90 | 0 |
| 802 | 2 |
| 24 | 4 |
| 2 | 2 |
| 66 | 6 |

Place the numbers in digit order, preserving their original order when units digits are equal:

- Digit 0: `170, 90`
- Digit 2: `802, 2`
- Digit 4: `24`
- Digit 5: `45, 75`
- Digit 6: `66`

After Pass 1:

`[170, 90, 802, 2, 24, 45, 75, 66]`

### Pass 2: Sort by Tens Digit

Read the tens digit of each number in the current order:

| Number | Tens digit |
|---|---:|
| 170 | 7 |
| 90 | 9 |
| 802 | 0 |
| 2 | 0 |
| 24 | 2 |
| 45 | 4 |
| 75 | 7 |
| 66 | 6 |

Group numbers by tens digit, preserving their current relative order within each group:

- Digit 0: `802, 2`
- Digit 2: `24`
- Digit 4: `45`
- Digit 6: `66`
- Digit 7: `170, 75`
- Digit 9: `90`

After Pass 2:

`[802, 2, 24, 45, 66, 170, 75, 90]`

### Pass 3: Sort by Hundreds Digit

Read the hundreds digit of each number in the current order:

| Number | Hundreds digit |
|---|---:|
| 802 | 8 |
| 2 | 0 |
| 24 | 0 |
| 45 | 0 |
| 66 | 0 |
| 170 | 1 |
| 75 | 0 |
| 90 | 0 |

Group by hundreds digit while maintaining the relative order within each group:

- Digit 0: `2, 24, 45, 66, 75, 90`
- Digit 1: `170`
- Digit 8: `802`

Final result:

`[2, 24, 45, 66, 75, 90, 170, 802]`

**The array is now sorted in ascending order.**

## 9.4 Pseudocode

```text
RadixSort(A, n)
    maxValue = maximum element in A
    exp = 1

    while floor(maxValue / exp) > 0
        StableSortByDigit(A, exp)
        exp = exp * 10
```

`StableSortByDigit` sorts according to the current digit:

`digit = floor(x / exp) mod 10`

For example, when `exp = 10`, the algorithm processes the tens digit.

## 9.5 Complexity Analysis

Let:
- `n` = number of elements.
- `d` = number of digit positions.
- `b` = base or number of possible digit values (10 for decimal digits).

| Measure | Complexity |
|---|---|
| Time with a stable linear-time digit sort | `O(d(n + b))` |
| Auxiliary space for a typical Counting Sort digit pass | `O(n + b)` |

If the base is fixed at 10, the time complexity is often written as `O(dn)`.

**Important:** Radix Sort is not generally `O(n log n)`. It avoids comparison sorting by processing digit positions. Its performance depends on the number of digits and the stable sorting method used.

## 9.6 Advantages

- Can be very efficient for large collections of fixed-length integers.
- Does not rely on comparing every pair of elements.
- Can outperform comparison sorts when the number of digits is limited.

## 9.7 Disadvantages

- Usually requires additional memory.
- Performance depends on digit count and base.
- Standard implementations need adaptation for negative numbers, variable-length keys, or other data types.

## 9.8 Applications

- Sorting IDs.
- Sorting integers and fixed-width numeric records.
- Some string-sorting tasks.
- Data processing where keys have a suitable digit or character representation.

---

# 10. Algorithm Comparison Tables

## 10.1 Searching Algorithms

| Feature | Linear Search | Binary Search |
|---|---|---|
| Input | Sorted or unsorted | Sorted |
| Best time | `O(1)` | `O(1)` |
| Average time | `O(n)` | `O(log n)` |
| Worst time | `O(n)` | `O(log n)` |
| Iterative auxiliary space | `O(1)` | `O(1)` |
| Main technique | Sequential checking | Halving the search interval |

## 10.2 Sorting Algorithms

| Algorithm | Best time | Average time | Worst time | Auxiliary space | Stable? |
|---|---|---|---|---|---|
| Bubble Sort | `O(n)`* | `O(n²)` | `O(n²)` | `O(1)` | Yes |
| Selection Sort | `O(n²)` | `O(n²)` | `O(n²)` | `O(1)` | Usually no |
| Insertion Sort | `O(n)` | `O(n²)` | `O(n²)` | `O(1)` | Yes |
| Bucket Sort | `O(n + k)`* | `O(n + k)`* | `O(n²)`* | `O(n + k)` | Depends on implementation |
| Radix Sort | `O(d(n + b))` | `O(d(n + b))` | `O(d(n + b))` | `O(n + b)` | Yes, with stable digit sorting |

**Notes:**
- `n` = number of elements.
- `k` = number of buckets.
- `d` = number of digit positions.
- `b` = digit base.
- `*` Bubble Sort's best case assumes an early-stop flag.
- `*` Bucket Sort's favorable bound assumes suitable distribution and an appropriate within-bucket sorting strategy. The quadratic worst case assumes a simple comparison sort in a heavily populated bucket.
- Radix Sort's stated bound assumes a stable, linear-time digit sort such as Counting Sort.

## 10.3 Important Sorting Terms

### Stable Sorting

A **stable sort** preserves the original relative order of elements that have equal keys.

Example records:

`(Ali, 80), (Sara, 70), (Usman, 80)`

If sorting by marks, a stable algorithm keeps Ali before Usman because both have marks of `80` and Ali appeared first.

### In-Place Sorting

An **in-place sort** uses only a small, constant amount of auxiliary memory, apart from the input array and implementation-specific details.

### Adaptive Sorting

An **adaptive sort** takes advantage of existing order in the input to reduce work.

| Algorithm | Stable | In-place | Adaptive in a common implementation |
|---|---|---|---|
| Bubble Sort | Yes | Yes | Yes, with early stop |
| Selection Sort | Usually no | Yes | No |
| Insertion Sort | Yes | Yes | Yes |
| Bucket Sort | Depends on implementation | Usually no | Depends on implementation |
| Radix Sort | Yes, with stable digit sorting | Usually no | Not generally |

---

# 11. Important Exam Questions

## 11.1 Short Questions

1. Define searching.
2. Define sorting.
3. What is Linear Search?
4. What is Binary Search?
5. Why must an array be sorted for ordinary Binary Search?
6. What is time complexity?
7. What is auxiliary space complexity?
8. Define Big-O notation.
9. What is a stable sorting algorithm?
10. What is an in-place sorting algorithm?
11. What is an adaptive sorting algorithm?
12. Which sorting algorithm repeatedly compares adjacent elements?
13. Which algorithm selects the minimum element from the unsorted portion?
14. Which algorithm inserts each key into a sorted prefix?
15. What is a bucket in Bucket Sort?
16. What is the least significant digit in Radix Sort?

## 11.2 Long Questions

1. Explain Linear Search and Binary Search with algorithms, examples, and complexity analysis.
2. Explain Bubble Sort with a complete pass-by-pass example.
3. Explain Selection Sort and discuss why its best-case complexity is still `O(n²)`.
4. Explain Insertion Sort and compare it with Bubble Sort.
5. Describe Bucket Sort, its distribution assumptions, and its time complexity.
6. Explain LSD Radix Sort using units, tens, and hundreds digits.
7. Differentiate between stable and unstable sorting algorithms.
8. Explain best-case, average-case, and worst-case time complexity.
9. Compare all five sorting algorithms in terms of time, space, stability, and applications.
10. Explain why Binary Search is faster than Linear Search for large sorted arrays.

## 11.3 Practice Problems

Try to solve these without looking at the worked examples.

### Problem 1: Linear Search

Array: `[8, 3, 15, 6, 12, 9]`  
Target: `12`

Tasks:
- Show each comparison.
- Give the target's index.
- State the number of comparisons.

### Problem 2: Binary Search

Sorted array: `[5, 10, 15, 20, 25, 30, 35, 40, 45]`  
Target: `35`

Tasks:
- Show `low`, `high`, and `mid` at every step.
- State the number of comparisons.

### Problem 3: Bubble Sort

Sort in ascending order:

`[9, 4, 7, 1, 5]`

Tasks:
- Show every pass.
- Count swaps.
- Give the final array.

### Problem 4: Selection Sort

Sort in ascending order:

`[29, 10, 14, 37, 13]`

Tasks:
- Show the minimum chosen in each pass.
- Show the array after every pass.

### Problem 5: Insertion Sort

Sort in ascending order:

`[8, 3, 5, 2, 9]`

Tasks:
- Show the key in each pass.
- Show all shifts and the final array.

### Problem 6: Bucket Sort

Sort:

`[0.62, 0.12, 0.83, 0.34, 0.25, 0.55]`

Tasks:
- Create suitable buckets.
- Distribute each value.
- Sort each bucket and concatenate the results.

### Problem 7: Radix Sort

Sort:

`[329, 457, 657, 839, 436, 720, 355]`

Tasks:
- Show the result after the units pass.
- Show the result after the tens pass.
- Show the result after the hundreds pass.

---

# 12. Quick Revision Sheet

## Linear Search

- Checks one element at a time.
- Works on sorted and unsorted data.
- Best time: `O(1)`.
- Average and worst time: `O(n)`.
- Auxiliary space: `O(1)`.

## Binary Search

- Repeatedly halves the search range.
- Requires sorted data.
- Best time: `O(1)`.
- Average and worst time: `O(log n)`.
- Iterative auxiliary space: `O(1)`.

## Bubble Sort

- Compares adjacent elements and swaps them.
- Largest unsorted value moves to the end after each pass.
- Best time with early stop: `O(n)`.
- Average and worst time: `O(n²)`.
- Auxiliary space: `O(1)`.
- Stable and in-place.

## Selection Sort

- Selects the minimum from the unsorted portion.
- Best, average, and worst time: `O(n²)`.
- Auxiliary space: `O(1)`.
- Usually unstable and in-place.
- Performs at most `n - 1` swaps in the standard version.

## Insertion Sort

- Inserts each key into the correct position in the sorted prefix.
- Best time: `O(n)`.
- Average and worst time: `O(n²)`.
- Auxiliary space: `O(1)`.
- Stable, in-place, and adaptive.

## Bucket Sort

- Distributes elements into buckets.
- Sorts each bucket and combines them.
- Can achieve expected `O(n + k)` time under suitable distribution assumptions.
- Worst-case time can be `O(n²)` with a simple comparison sort inside one heavily populated bucket.
- Usually requires extra space.

## Radix Sort

- Sorts values by individual digit positions.
- LSD version processes units, tens, hundreds, and so on.
- With stable linear-time digit sorting, time is `O(d(n + b))`.
- Typical auxiliary space: `O(n + b)`.
- Does not use ordinary element-to-element comparisons as its primary sorting method.

---

## Final Exam Preparation Advice

For each algorithm, practice writing four things from memory:

1. **Definition**
2. **Pseudocode**
3. **Complete step-by-step example**
4. **Complexity analysis**

For sorting algorithms, show the array after every pass. For searching algorithms, show each comparison and the final index or “Not Found” result. This format prepares you for both theory questions and tracing problems.
