# Algorithm Design Approaches --- Complete Notes

## Table of Contents

1.  [Introduction](#1-introduction)
2.  [Brute Force Approach](#2-brute-force-approach)
3.  [Divide-and-Conquer Approach](#3-divide-and-conquer-approach)
4.  [Greedy Approach](#4-greedy-approach)
5.  [Comparison of the Three
    Approaches](#5-comparison-of-the-three-approaches)
6.  [Quick Revision Notes](#6-quick-revision-notes)
7.  [Exam Preparation Questions](#7-exam-preparation-questions)

------------------------------------------------------------------------

## 1. Introduction

An **algorithm design approach** is a general strategy used to develop
an algorithm for solving a problem. Different approaches solve problems
in different ways, and each has strengths and limitations.

This document covers three important approaches:

-   **Brute Force:** Try all possible candidates or follow the most
    direct method.
-   **Divide and Conquer:** Break a problem into smaller subproblems,
    solve them, and combine their results.
-   **Greedy:** Make the best-looking choice at each step, hoping to
    construct a good overall solution.

### Why do we study algorithm design approaches?

They help us:

-   Choose a suitable method for a problem.
-   Understand the time and space required by an algorithm.
-   Compare different solutions.
-   Design programs that are easier to analyze and improve.

------------------------------------------------------------------------

## 2. Brute Force Approach

### 2.1 Definition

The **Brute Force Approach** solves a problem using a direct,
straightforward method. Depending on the problem, it may test every
possible candidate until it finds a solution or determine the answer by
checking all possibilities.

It is often easy to understand and implement, but it can be slow when
the number of possible candidates is large.

### 2.2 Main idea

1.  Understand the problem.
2.  Generate or examine possible candidates.
3.  Check each candidate against the required condition.
4.  Return a valid answer or the best answer found.

### 2.3 Example 1: Linear Search

Suppose we want to find `7` in the array:

`[4, 9, 2, 7, 5]`

Check the elements one by one:

    Step   Element checked Result
  ------ ----------------- -----------
       1                 4 Not found
       2                 9 Not found
       3                 2 Not found
       4                 7 Found

The target is found at index `3` (the fourth position, using zero-based
indexing).

**Time complexity:** - Best case: `O(1)` --- the first element is the
target. - Average case: `O(n)` --- a typical search examines a portion
of the array. - Worst case: `O(n)` --- the target is last or absent.

**Auxiliary space:** `O(1)` for the usual iterative implementation.

### 2.4 Example 2: Find a pair with a target sum

Given the array `[2, 7, 4, 9]`, find two different elements whose sum is
`11`.

A brute-force solution checks pairs:

1.  `2 + 7 = 9` --- not 11.
2.  `2 + 4 = 6` --- not 11.
3.  `2 + 9 = 11` --- found.

The answer is the pair `(2, 9)`.

#### Pseudocode

``` text
BruteForcePairSum(A, target):
    for i = 0 to length(A) - 2:
        for j = i + 1 to length(A) - 1:
            if A[i] + A[j] == target:
                return (A[i], A[j])
    return "No pair found"
```

**Complexity:** - Time: `O(n²)` in the worst case because the algorithm
examines pairs. - Auxiliary space: `O(1)`.

### 2.5 Advantages

-   Simple to understand.
-   Usually straightforward to implement.
-   Often useful for small input sizes.
-   Can provide a baseline for comparing optimized algorithms.
-   If all possibilities are examined correctly, it can find an exact
    solution.

### 2.6 Disadvantages

-   Can be very slow for large inputs.
-   May repeat unnecessary work.
-   Exhaustive search can become impractical when possibilities grow
    exponentially or factorially.

### 2.7 Applications

-   Linear search.
-   Checking all pairs or combinations.
-   Simple password or key-space demonstrations (only in authorized
    educational contexts).
-   Testing small problem instances.
-   Finding a simple reference solution for validating a faster
    algorithm.

### 2.8 When should we use it?

Use brute force when the input is small, the problem is simple, a clear
baseline is needed, or no more efficient method is known. Before using
it for large inputs, estimate how many candidates it will examine.

------------------------------------------------------------------------

## 3. Divide-and-Conquer Approach

### 3.1 Definition

**Divide and Conquer** is an algorithm design approach that divides a
large problem into smaller subproblems of the same or a related type,
solves those subproblems, and combines their results to solve the
original problem.

### 3.2 Three main steps

1.  **Divide:** Split the original problem into smaller subproblems.
2.  **Conquer:** Solve the subproblems, often recursively. Small enough
    cases are handled directly.
3.  **Combine:** Merge or use the subproblem results to obtain the final
    answer.

### 3.3 Example 1: Binary Search

Binary Search finds a target in a **sorted** array by repeatedly
checking the middle element and discarding the half that cannot contain
the target.

Find `70` in:

`[10, 20, 30, 40, 50, 60, 70, 80, 90]`

Step-by-step:

1.  Low index = 0, high index = 8. Middle index = 4; value = `50`.
    -   `70 > 50`, so continue in the right half.
2.  Low index = 5, high index = 8. Middle index = 6; value = `70`.
    -   Target found at index `6`.

The target is found after two middle-element checks.

#### Pseudocode

``` text
BinarySearch(A, target):
    low = 0
    high = length(A) - 1

    while low <= high:
        mid = low + floor((high - low) / 2)

        if A[mid] == target:
            return mid
        else if A[mid] < target:
            low = mid + 1
        else:
            high = mid - 1

    return -1
```

**Complexity:** - Best case: `O(1)`. - Average and worst case:
`O(log n)`. - Auxiliary space: `O(1)` for this iterative version.

**Important:** Binary Search requires the input to be sorted.

### 3.4 Example 2: Merge Sort

Merge Sort sorts an array by dividing it into halves, sorting each half,
and merging the sorted halves.

Sort `[8, 3, 6, 2]`.

#### Step 1: Divide

``` text
[8, 3, 6, 2]
       /   \
   [8, 3] [6, 2]
    / \     / \
  [8] [3] [6] [2]
```

#### Step 2: Conquer

Single-element arrays are already sorted.

``` text
[8] [3] [6] [2]
```

#### Step 3: Combine by merging

Merge `[8]` and `[3]`:

``` text
[3, 8]
```

Merge `[6]` and `[2]`:

``` text
[2, 6]
```

Finally, merge `[3, 8]` and `[2, 6]`:

-   Compare 3 and 2 → take 2.
-   Compare 3 and 6 → take 3.
-   Compare 8 and 6 → take 6.
-   Append 8.

Final sorted array:

`[2, 3, 6, 8]`

#### Pseudocode

``` text
MergeSort(A):
    if length(A) <= 1:
        return A

    divide A into left and right halves
    leftSorted = MergeSort(left)
    rightSorted = MergeSort(right)

    return Merge(leftSorted, rightSorted)
```

The `Merge` procedure repeatedly takes the smaller next element from the
two sorted halves and appends any remaining elements.

**Complexity:** - Best, average, and worst-case time: `O(n log n)` for
standard Merge Sort. - Auxiliary space: typically `O(n)` for array-based
Merge Sort.

### 3.5 Advantages

-   Often much faster than checking every possibility.
-   Breaks complex problems into manageable pieces.
-   Naturally supports recursive solutions.
-   Provides efficient algorithms such as Merge Sort and Binary Search.

### 3.6 Disadvantages

-   Recursion can add function-call overhead.
-   Combining results may require extra memory or processing.
-   It can be more complicated to implement than a direct solution.
-   Not every problem can be divided into useful independent
    subproblems.

### 3.7 Applications

-   Binary Search.
-   Merge Sort.
-   Quick Sort (a divide-and-conquer sorting algorithm).
-   Finding minimum and maximum using recursive splitting.
-   Matrix multiplication algorithms.
-   Some computational geometry algorithms.

### 3.8 When should we use it?

Use Divide and Conquer when a problem can be split into smaller
subproblems that are easier to solve, and their solutions can be
combined efficiently. It is especially effective when the total work
across levels remains manageable.

------------------------------------------------------------------------

## 4. Greedy Approach

### 4.1 Definition

The **Greedy Approach** constructs a solution step by step. At each
step, it chooses what seems to be the best immediate option according to
a rule, without reconsidering earlier choices.

A greedy choice is locally optimal, but it does **not** always guarantee
a globally optimal solution. Whether it works depends on the structure
of the problem.

### 4.2 Main idea

1.  Identify the available choices.
2.  Define a rule for selecting the best immediate choice.
3.  Choose the best available option.
4.  Add it to the solution if it is valid.
5.  Repeat until the solution is complete.

### 4.3 Example 1: Coin Change

Suppose the available coin denominations are:

`[25, 10, 5, 1]`

Make a total of `41` using as few coins as possible. Using the largest
denomination that does not exceed the remaining amount:

1.  Choose 25. Remaining amount: `41 - 25 = 16`.
2.  Choose 10. Remaining amount: `16 - 10 = 6`.
3.  Choose 5. Remaining amount: `6 - 5 = 1`.
4.  Choose 1. Remaining amount: `1 - 1 = 0`.

Result:

`41 = 25 + 10 + 5 + 1`

Total coins used: **4**.

For this denomination system, the greedy result is optimal for this
amount. However, greedy coin change is **not** optimal for every
possible denomination system. For example, with denominations
`[1, 3, 4]` and target `6`, greedy picks `4 + 1 + 1` (3 coins), while
the optimal solution is `3 + 3` (2 coins).

### 4.4 Example 2: Activity Selection

Choose the maximum number of non-overlapping activities. Each activity
has a start time and a finish time.

  Activity     Start   Finish
  ---------- ------- --------
  A                1        3
  B                2        5
  C                4        6
  D                6        8
  E                5        7

A standard greedy rule is to select the activity that finishes earliest
among the compatible remaining activities.

Assume activities are sorted by finish time:

1.  Select A `(1, 3)` because it finishes earliest.
2.  B `(2, 5)` overlaps A, so skip it.
3.  Select C `(4, 6)` because it starts after A finishes.
4.  E `(5, 7)` overlaps C, so skip it.
5.  Select D `(6, 8)` because it starts when C finishes.

Selected activities: **A, C, D**.

Total selected: **3 activities**.

#### Pseudocode

``` text
ActivitySelection(activities):
    sort activities by increasing finish time
    selected = empty list
    lastFinish = -infinity

    for each activity in activities:
        if activity.start >= lastFinish:
            add activity to selected
            lastFinish = activity.finish

    return selected
```

**Complexity:** Sorting takes `O(n log n)`; scanning takes `O(n)`. Total
time is `O(n log n)` when sorting is required, or `O(n)` if activities
are already sorted. The storage for the selected list can be up to
`O(n)`.

### 4.5 Conditions that help greedy algorithms work

A greedy strategy is appropriate when the problem has properties that
justify making a locally optimal choice. Two commonly discussed
properties are:

-   **Greedy-choice property:** There is an optimal solution that begins
    with the greedy choice.
-   **Optimal substructure:** After making a valid choice, the remaining
    problem can be solved optimally as a smaller instance of the same
    problem.

These properties need to be established for the specific problem; they
should not simply be assumed.

### 4.6 Advantages

-   Often simple to design and implement.
-   Can be efficient in time and memory.
-   Makes decisions without exploring every possible solution.
-   Works well for problems with a proven greedy-choice property.

### 4.7 Disadvantages

-   Does not guarantee an optimal answer for every problem.
-   Earlier decisions are usually not reconsidered.
-   Choosing a suitable greedy rule can be difficult.
-   A solution may look reasonable at each step but still be worse
    overall.

### 4.8 Applications

-   Activity Selection.
-   Fractional Knapsack.
-   Minimum Spanning Tree algorithms such as Kruskal's and Prim's.
-   Dijkstra's shortest-path algorithm when edge weights are
    nonnegative.
-   Huffman Coding.
-   Some coin-change systems.

**Important distinction:** The greedy strategy solves Fractional
Knapsack optimally by selecting items according to value per unit
weight. The same simple strategy does not generally solve 0/1 Knapsack
optimally.

### 4.9 When should we use it?

Use Greedy when a locally optimal choice can be shown to lead to a
globally optimal solution. Test the rule carefully and look for
counterexamples before assuming it is correct.

------------------------------------------------------------------------

## 5. Comparison of the Three Approaches

  -----------------------------------------------------------------------
  Feature           Brute Force       Divide and        Greedy
                                      Conquer           
  ----------------- ----------------- ----------------- -----------------
  Basic idea        Try candidates    Divide, solve,    Choose the best
                    directly          combine           immediate option

  Main strategy     Direct or         Recursive         Step-by-step
                    exhaustive        decomposition     selection
                    checking                            

  Looks ahead?      May examine many  Solves smaller    Usually does not
                    possibilities     subproblems       reconsider
                                                        earlier choices

  Optimal answer    Yes if the search Depends on the    Only when the
                    correctly covers  correctness of    greedy rule is
                    all possibilities the algorithm     valid for the
                    and the objective                   problem
                    is evaluated                        
                    correctly                           

  Typical example   Linear Search;    Merge Sort;       Activity
                    checking all      Binary Search     Selection;
                    pairs                               Fractional
                                                        Knapsack

  Common drawback   Can be too slow   May need          Can fail to find
                                      recursion and     the global
                                      extra memory      optimum
  -----------------------------------------------------------------------

### Key differences in simple language

-   **Brute Force:** "Try the possibilities."
-   **Divide and Conquer:** "Break the problem into smaller problems."
-   **Greedy:** "Choose the best-looking option now."

------------------------------------------------------------------------

## 6. Quick Revision Notes

### Brute Force

-   Direct, straightforward method.
-   May check every candidate.
-   Easy to understand and implement.
-   Often inefficient for large inputs.
-   Example: checking every pair to find a target sum.
-   Pair-sum nested loops: `O(n²)` time, `O(1)` auxiliary space.

### Divide and Conquer

-   Three steps: Divide, Conquer, Combine.
-   Often uses recursion.
-   Example: Merge Sort.
-   Merge Sort time: `O(n log n)`.
-   Array-based Merge Sort typically uses `O(n)` auxiliary space.
-   Binary Search time: `O(log n)`; input must be sorted.

### Greedy

-   Makes the best immediate valid choice.
-   Usually does not revisit earlier choices.
-   Can be fast and simple.
-   Does not always give an optimal result.
-   Example: Activity Selection.
-   Activity Selection takes `O(n log n)` time when sorting is needed.

### Memory trick

-   **Brute Force:** Try.
-   **Divide and Conquer:** Split.
-   **Greedy:** Choose.

------------------------------------------------------------------------

## 7. Exam Preparation Questions

### Short questions

1.  Define the Brute Force Approach.
2.  What is Divide and Conquer?
3.  Name the three stages of Divide and Conquer.
4.  Define the Greedy Approach.
5.  Why does Binary Search require sorted data?
6.  Does a greedy algorithm always produce an optimal solution? Explain
    briefly.
7.  Give two examples of algorithms that use Divide and Conquer.
8.  Give two examples of greedy algorithms.

### Long questions

1.  Explain the Brute Force Approach with an example, pseudocode, and
    complexity analysis.
2.  Explain Divide and Conquer and demonstrate Merge Sort step by step.
3.  Explain the Greedy Approach using Activity Selection.
4.  Compare Brute Force, Divide and Conquer, and Greedy approaches.
5.  Explain why the greedy coin-change strategy can fail for some
    denomination systems.

### Practice tasks

1.  Use Linear Search to find `12` in `[5, 8, 12, 17, 21]`.
2.  Use Binary Search to find `35` in `[5, 10, 15, 20, 25, 30, 35, 40]`.
3.  Show every merge step when sorting `[7, 2, 9, 1]` using Merge Sort.
4.  Use the greedy coin strategy with denominations `[25, 10, 5, 1]` to
    make `37`.
5.  Explain why greedy coin change with denominations `[1, 3, 4]` fails
    to give the fewest coins for target `6`.

------------------------------------------------------------------------

## Final Summary

Algorithm design approaches provide different ways to solve
computational problems:

-   **Brute Force** uses a direct method and may inspect all
    possibilities.
-   **Divide and Conquer** divides a problem into smaller subproblems
    and combines their results.
-   **Greedy** makes locally best choices and is optimal only for
    problems where the greedy-choice rule is valid.

The best approach depends on the problem, input size, correctness
requirements, and performance needs.
