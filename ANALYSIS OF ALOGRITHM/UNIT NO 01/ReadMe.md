# Introduction to Algorithms — Complete Study Notes

**Subject:** Computer Science  
**Topics covered:** Introduction to Algorithms; Role of Algorithms in Computing; Nature and Size of Input; Principles of Algorithm Analysis.

---

## Table of Contents

1. [Introduction to Algorithms](#1-introduction-to-algorithms)
2. [Role of Algorithms in Computing](#2-role-of-algorithms-in-computing)
3. [Nature of Input and Size of Input](#3-nature-of-input-and-size-of-input)
4. [Principles of Algorithm Analysis](#4-principles-of-algorithm-analysis)
5. [Worked Examples](#5-worked-examples)
6. [Quick Revision Notes](#6-quick-revision-notes)
7. [Important Exam Questions](#7-important-exam-questions)

---

## 1. Introduction to Algorithms

### 1.1 What is an algorithm?

An **algorithm** is a finite sequence of clear, well-defined instructions used to solve a problem or perform a computation.

In simple words, an algorithm tells a computer **what steps to perform and in what order** to obtain the desired result.

Examples include:

- Calculating the average of students' marks.
- Searching for a name in a list.
- Sorting numbers in ascending order.
- Finding the shortest route between two cities.
- Encrypting a message.

### 1.2 Basic characteristics of an algorithm

A good algorithm generally has the following characteristics.

1. **Input:** An algorithm accepts zero or more input values. For example, two numbers used in addition.
2. **Output:** It produces at least one meaningful result, such as the sum of two numbers.
3. **Definiteness:** Every instruction must be precise and unambiguous. “Add 5 to x” is clear; “change x appropriately” is not.
4. **Finiteness:** The algorithm must terminate after a finite number of steps for every valid input covered by its specification.
5. **Effectiveness:** Each operation must be basic enough to carry out in practice, in the assumed computational model.
6. **Correctness:** The algorithm must produce the required result for every valid input, according to the problem specification.

### 1.3 Algorithm vs. program

An algorithm and a program are related, but they are not the same.

| Algorithm | Program |
|---|---|
| A step-by-step solution to a problem. | An implementation of a solution in a programming language. |
| Can be written in plain language, pseudocode, or a flowchart. | Written in languages such as Python, C++, or Java. |
| Independent of a particular programming language. | Uses the syntax and rules of a particular language. |
| Focuses on the logic of the solution. | Focuses on executable instructions and implementation details. |

**Example:** The method for finding the largest number in a list is an algorithm. Its implementation in Python is a program.

### 1.4 Ways to represent an algorithm

Algorithms can be described in several forms:

1. **Natural language:** Steps explained using ordinary words.
2. **Pseudocode:** Language-independent, structured instructions resembling code.
3. **Flowchart:** A graphical representation using symbols and arrows.
4. **Programming language:** Actual code that can be executed by a computer.

### 1.5 Example: Algorithm to add two numbers

**Problem:** Find the sum of two input numbers.

```text
ALGORITHM AddTwoNumbers
    INPUT A, B
    SUM ← A + B
    OUTPUT SUM
END
```

Example: If `A = 10` and `B = 20`, the output is `30`.

The algorithm accepts two inputs, performs one addition, produces an output, and terminates.

### 1.6 Stages of algorithm development

The general process of developing an algorithm is:

1. **Understand the problem:** Identify the objective and requirements.
2. **Identify inputs and outputs:** Determine what is given and what must be produced.
3. **Design the algorithm:** Choose a suitable sequence of steps.
4. **Verify correctness:** Check whether the steps solve the problem.
5. **Analyze efficiency:** Estimate time and memory requirements.
6. **Implement and test:** Write the program and test different cases.

---

## 2. Role of Algorithms in Computing

Algorithms are the foundation of computing. Computers execute instructions, but algorithms organize those instructions into logical steps that solve useful problems.

### 2.1 Why are algorithms important?

1. **Problem-solving:** Algorithms break complex problems into smaller, manageable steps.
2. **Efficiency:** They help minimize execution time and memory usage.
3. **Automation:** Repeated tasks can be performed automatically and consistently.
4. **Accuracy:** Correct algorithms reduce mistakes and produce predictable results.
5. **Scalability:** Efficient algorithms can handle larger inputs without requiring impractical amounts of time or memory.
6. **Resource management:** Algorithms help make effective use of CPU time, memory, storage, and network resources.
7. **Software development:** They provide the logical foundation for implementing reliable applications.

### 2.2 Applications of algorithms

#### 1. Searching and information retrieval

Search algorithms locate a particular item in a collection. For example, binary search finds a value in a sorted array in logarithmic time.

#### 2. Sorting data

Sorting algorithms arrange data into a specified order. They are used in databases, reports, and online shopping filters.

#### 3. Navigation and networks

Graph algorithms find routes and analyze connections between locations, computers, or people.

#### 4. Security and cryptography

Cryptographic algorithms support encryption, digital signatures, authentication, and secure communication.

#### 5. Artificial intelligence and machine learning

Algorithms train models, classify data, make predictions, and support automated decision-making.

#### 6. Databases and operating systems

Algorithms organize records, schedule processes, manage memory, and allocate computing resources.

### 2.3 Correctness versus efficiency

Two major questions are asked when evaluating an algorithm:

- **Correctness:** Does it produce the required output?
- **Efficiency:** How much time and memory does it require?

An algorithm may be correct but inefficient. For example, an algorithm that compares every pair of elements to find a duplicate can work correctly but take quadratic time.

An efficient algorithm that produces incorrect results is not acceptable either. Therefore, **correctness and efficiency are both essential**.

### 2.4 Comparing two algorithms

Suppose two algorithms solve the same problem:

- Algorithm A performs approximately \(n^2\) operations.
- Algorithm B performs approximately \(n\log_2 n\) operations.

For \(n = 1{,}000\):

- Algorithm A: about 1,000,000 operations.
- Algorithm B: about 9,966 operations.

Algorithm B performs far fewer operations in this example. This difference becomes increasingly important as the input grows. These are illustrative operation counts; actual running time depends on implementation and hardware.

### 2.5 Trade-offs in algorithm design

Sometimes improving one feature makes another worse.

- **Time vs. space:** An algorithm may use extra memory to reduce execution time.
- **Simplicity vs. performance:** A simple algorithm may be easier to implement but slower on large inputs.
- **Exactness vs. approximation:** An approximate method may provide a useful answer much faster than an exact method.
- **Preprocessing vs. repeated queries:** Spending time organizing data first can make future searches faster.

**Example:** A hash table generally requires extra memory, but it can provide average-case constant-time lookups under suitable assumptions.

### 2.6 Importance of algorithm analysis

Algorithm analysis allows developers to:

1. Predict performance before implementation.
2. Compare alternative solutions objectively.
3. Estimate whether a solution can handle large datasets.
4. Identify bottlenecks.
5. Choose appropriate algorithms for real-world constraints.

---

## 3. Nature of Input and Size of Input

The nature of input and its size are important because an algorithm's performance depends not only on how many values it receives but also on the characteristics and arrangement of those values.

### 3.1 What is input?

**Input** is the data supplied to an algorithm before or during its execution.

Examples include:

- A number whose factorial must be calculated.
- An array that must be sorted.
- A word that must be searched for.
- A graph representing roads between cities.
- A collection of database records.

The input determines which operations an algorithm performs and how much work may be required.

### 3.2 What is input size?

**Input size** is a measure of the amount of data given to an algorithm. It is usually represented by the variable \(n\), but the appropriate measure depends on the problem.

| Problem | Possible input-size measure |
|---|---|
| Searching an array | Number of elements, \(n\) |
| Sorting a list | Number of elements, \(n\) |
| Processing a text document | Number of characters, \(n\) |
| Multiplying integers | Number of bits in each integer |
| Processing a graph | Number of vertices \(V\) and edges \(E\) |
| Processing a matrix | Number of rows and columns, \(r\) and \(c\) |
| Processing a file | Number of bytes or records |

**Important:** Input size does not always mean the numerical value of the input. For example, the integer 1,000,000 has a binary representation containing 20 bits. Some algorithms depend on the integer's value, while others depend on its representation length.

### 3.3 Why does input size matter?

Input size is important for several reasons:

1. **Determines workload:** Larger inputs often require more operations.
2. **Helps estimate execution time:** Complexity analysis expresses growth as input size increases.
3. **Supports algorithm comparison:** It shows which algorithm is more suitable for large datasets.
4. **Helps predict scalability:** It indicates whether a solution will remain practical as data grows.
5. **Influences memory requirements:** Larger inputs may require more storage.

For example, if a linear search checks each element once, doubling the list size can approximately double the work in the worst case.

### 3.4 Nature of input

The *nature* of input refers to its properties, structure, distribution, and arrangement. Two inputs of equal size may require different amounts of work.

Important aspects include:

#### A. Input values

The actual values may affect the number of operations.

Example: Searching for the number 5 in a list can terminate immediately if 5 is the first element, but may examine the entire list if 5 is absent.

#### B. Input order

The arrangement of data can affect performance.

Example: Insertion sort performs well on an already sorted array but may perform much more work on a reverse-sorted array.

#### C. Input distribution

Data may be random, sorted, clustered, uniformly distributed, or concentrated around particular values. Certain algorithms perform differently for different distributions.

#### D. Special properties

An algorithm may exploit particular characteristics of the input, such as:

- Data being sorted.
- A graph having no cycles.
- A matrix containing mostly zeros.
- A sequence containing many repeated values.

#### E. Validity of input

Inputs may be valid or invalid according to the problem specification. A robust program should handle invalid inputs appropriately, although standard algorithm analysis usually assumes inputs satisfy the stated preconditions.

### 3.5 Input size versus input nature

| Input size | Input nature |
|---|---|
| Measures how much data is supplied. | Describes the properties of that data. |
| Often measured by \(n\), \(V\), \(E\), or bit length. | Includes order, values, distribution, and structure. |
| Usually determines the scale of analysis. | Often determines how much work is done for a particular input. |
| Example: an array contains 100 elements. | Example: those 100 elements are already sorted. |

Both factors are relevant to understanding algorithm performance.

### 3.6 Best-case, average-case, and worst-case input

An algorithm can perform differently on different inputs of the same size. Three common cases are used to describe this behavior.

#### Best case

The **best case** is the minimum work for input size \(n\), for the most favorable valid input.

Example: Linear search finds the target in the first position, requiring one comparison.

#### Average case

The **average case** is the expected work under a specified probability distribution over inputs.

Example: Linear search over a uniformly likely target position requires approximately half the array's elements to be examined.

#### Worst case

The **worst case** is the maximum work among valid inputs of size \(n\).

Example: Linear search finds the target at the last position or does not find it at all, requiring \(n\) comparisons.

### 3.7 Example: Linear search

Suppose an array contains \(n\) elements, and we want to find a target value `x`.

```text
ALGORITHM LinearSearch(A, n, x)
    FOR i ← 0 TO n - 1
        IF A[i] = x
            RETURN i
    RETURN -1
END
```

Here, `-1` means that the target was not found.

Consider the array:

`A = [10, 20, 30, 40, 50]`

- Searching for 10: one comparison.
- Searching for 30: three comparisons.
- Searching for 50: five comparisons.
- Searching for 99: five comparisons.

For an array of size \(n\):

| Case | Number of comparisons |
|---|---:|
| Best case | \(1\) |
| Average case, successful search with uniformly likely position | \((n+1)/2\) |
| Worst case | \(n\) |

Thus, linear search has best-case time \(\Theta(1)\) and worst-case time \(\Theta(n)\). Its average-case time is also \(\Theta(n)\) under the stated successful-search assumption.

### 3.8 Measuring input size for different problems

**Example 1: Array processing**

If an array contains 500 integers, its input size is \(n=500\).

**Example 2: Graph processing**

A graph may have 100 vertices and 300 edges. Its size is often expressed using both \(V=100\) and \(E=300\), because the cost of a graph algorithm may depend on both.

**Example 3: String processing**

If an algorithm checks every character in a string containing 2,000 characters, its input size is \(n=2{,}000\).

**Example 4: Integer arithmetic**

For an integer represented using \(b\) bits, arithmetic complexity is often analyzed in terms of \(b\), rather than the numerical value alone.

### 3.9 Relationship between input size and performance

The number of operations required by an algorithm is expressed as a function of input size, such as:

\[
T(n)=3n+5
\]

This represents a running-time model in which work grows linearly with \(n\).

For \(n=10\):

\[
T(10)=3(10)+5=35
\]

For \(n=100\):

\[
T(100)=3(100)+5=305
\]

As input size increases, the work increases according to the function. In asymptotic analysis, the constant factor and lower-order term are ignored, so \(3n+5\) is classified as \(\Theta(n)\).

---

## 4. Principles of Algorithm Analysis

### 4.1 What is algorithm analysis?

**Algorithm analysis** is the process of evaluating an algorithm's resource requirements, especially the time it takes to execute and the memory it uses, as a function of input size.

It helps determine whether an algorithm is efficient and suitable for a particular problem.

There are two main resources to analyze:

1. **Time complexity:** How the amount of computational work grows with input size.
2. **Space complexity:** How the memory requirement grows with input size.

Algorithm analysis generally focuses on growth rates rather than exact execution times.

### 4.2 Objectives of algorithm analysis

The main objectives are:

- To estimate the time and memory needed by an algorithm.
- To compare different algorithms that solve the same problem.
- To identify inefficient operations.
- To predict performance on large inputs.
- To choose an appropriate algorithm for a particular application.
- To understand the theoretical limits of a solution.

### 4.3 Time complexity

**Time complexity** describes how the running time or number of basic operations grows as input size \(n\) increases.

Instead of measuring seconds on a specific computer, theoretical analysis often counts basic operations, such as comparisons, assignments, arithmetic operations, or iterations.

Consider this example:

```text
ALGORITHM PrintNumbers(n)
    FOR i ← 1 TO n
        PRINT i
END
```

The loop executes \(n\) times. If each iteration takes constant time, the total running time grows linearly with \(n\).

Therefore,

\[
T(n)=an+b
\]

for suitable constants \(a\) and \(b\), and the complexity is \(\Theta(n)\).

### 4.4 Space complexity

**Space complexity** describes how the memory required by an algorithm grows with input size.

It can include:

- Memory occupied by input data, depending on the convention used.
- Variables and temporary storage.
- Auxiliary arrays and data structures.
- Recursion stack space.
- Other memory required during execution.

For example:

```text
ALGORITHM SumArray(A, n)
    SUM ← 0
    FOR i ← 1 TO n
        SUM ← SUM + A[i]
    RETURN SUM
END
```

The algorithm uses a fixed number of extra variables, regardless of \(n\). Its **auxiliary space complexity** is therefore \(\Theta(1)\), excluding the input array.

By contrast, creating a new array of size \(n\) generally requires \(\Theta(n)\) auxiliary space.

### 4.5 Time complexity versus space complexity

| Time complexity | Space complexity |
|---|---|
| Measures growth in computational work. | Measures growth in memory requirements. |
| Often depends on comparisons, loops, and recursive calls. | Often depends on variables, arrays, data structures, and recursion. |
| Helps estimate how execution cost scales. | Helps estimate how memory cost scales. |
| Example: linear search has \(O(n)\) worst-case time. | Iterative linear search uses \(O(1)\) auxiliary space. |

An algorithm may use more memory to run faster, or use less memory at the cost of additional computation.

### 4.6 Asymptotic analysis

Asymptotic analysis studies how an algorithm's resource requirements grow as the input size becomes large.

It abstracts away hardware-specific details and focuses on the dominant growth rate.

Three important notations are:

- Big O: \(O\)
- Big Omega: \(\Omega\)
- Big Theta: \(\Theta\)

#### A. Big O notation — \(O\)

Big O provides an asymptotic upper bound on a function's growth.

Formally, \(f(n)=O(g(n))\) if there are positive constants \(c\) and \(n_0\) such that

\[
0\le f(n)\le c\,g(n),\quad n\ge n_0.
\]

In introductory algorithm analysis, Big O is commonly used to express an algorithm's worst-case growth, although the notation itself is not limited to worst-case analysis.

Example:

\[
f(n)=3n^2+5n+2
\]

Its asymptotic upper bound is \(O(n^2)\).

#### B. Big Omega notation — \(\Omega\)

Big Omega provides an asymptotic lower bound.

Formally, \(f(n)=\Omega(g(n))\) if there are positive constants \(c\) and \(n_0\) such that

\[
0\le c\,g(n)\le f(n),\quad n\ge n_0.
\]

For example,

\[
3n^2+5n+2=\Omega(n^2).
\]

This means the function grows at least as fast as a constant multiple of \(n^2\), for sufficiently large \(n\).

#### C. Big Theta notation — \(\Theta\)

Big Theta gives a tight asymptotic bound: the function is bounded both above and below by constant multiples of the same function.

Formally,

\[
f(n)=\Theta(g(n))
\]

when \(f(n)=O(g(n))\) and \(f(n)=\Omega(g(n))\).

For example,

\[
3n^2+5n+2=\Theta(n^2).
\]

This means its growth rate is quadratic.

### 4.7 Common orders of growth

The following table shows common complexity classes, from slower growth to faster growth.

| Complexity | Name | Typical example |
|---|---|---|
| \(O(1)\) | Constant | Accessing an array element by index |
| \(O(\log n)\) | Logarithmic | Binary search |
| \(O(n)\) | Linear | Linear search |
| \(O(n\log n)\) | Linearithmic | Merge sort |
| \(O(n^2)\) | Quadratic | Simple nested-loop comparisons |
| \(O(n^3)\) | Cubic | A typical three-nested-loop computation |
| \(O(2^n)\) | Exponential | Enumerating all subsets |
| \(O(n!)\) | Factorial | Enumerating all permutations |

These are typical examples; the precise complexity depends on the algorithm and the operations being analyzed.

### 4.8 Rules for calculating time complexity

Several basic rules help analyze algorithms.

#### Rule 1: Constant-time statements

A fixed number of basic operations takes constant time.

```text
A ← 10
B ← 20
C ← A + B
```

Complexity: \(\Theta(1)\).

#### Rule 2: Single loop

```text
FOR i ← 1 TO n
    PRINT i
```

The loop runs \(n\) times.

Complexity: \(\Theta(n)\).

#### Rule 3: Nested loops

```text
FOR i ← 1 TO n
    FOR j ← 1 TO n
        PRINT i, j
```

The inner loop executes \(n\) times for each of the \(n\) outer iterations.

Total operations:

\[
n\times n=n^2
\]

Complexity: \(\Theta(n^2)\).

#### Rule 4: Consecutive loops

```text
FOR i ← 1 TO n
    PRINT i

FOR j ← 1 TO n
    PRINT j
```

Total work is \(n+n=2n\), so the complexity is \(\Theta(n)\).

#### Rule 5: Repeated halving

```text
WHILE n > 1
    n ← n / 2
```

The number of iterations is approximately \(\log_2 n\).

Complexity: \(\Theta(\log n)\).

#### Rule 6: Independent nested loops

If the outer loop runs \(n\) times and the inner loop runs \(m\) times, the complexity is \(\Theta(nm)\), assuming constant work per iteration.

### 4.9 Best, average, and worst-case analysis

These concepts can be applied to time and other computational resources.

- **Best-case analysis:** Finds the minimum resource usage among inputs of size \(n\).
- **Average-case analysis:** Estimates expected resource usage under a specified input distribution.
- **Worst-case analysis:** Finds the maximum resource usage among inputs of size \(n\).

For linear search:

| Case | Example | Time |
|---|---|---|
| Best | Target is first | \(\Theta(1)\) |
| Average | Target position is uniformly distributed | \(\Theta(n)\) |
| Worst | Target is last or absent | \(\Theta(n)\) |

Worst-case analysis is particularly useful when performance guarantees are important, because it gives an upper bound on the work required for any valid input of a given size.

### 4.10 Principles of good algorithm analysis

The following principles make an analysis more reliable and useful.

1. **Define the problem precisely.** Identify the required output and valid inputs.
2. **Define the input size.** Choose an appropriate measure, such as array length or graph size.
3. **Choose a computational model.** State what operations are counted and whether they are assumed to take constant time.
4. **Identify the dominant operations.** Count comparisons, loop iterations, recursive calls, or other relevant operations.
5. **Consider different input cases.** Analyze best-case, average-case, and worst-case behavior where appropriate.
6. **Use asymptotic notation.** Describe growth using \(O\), \(\Omega\), and \(\Theta\).
7. **Analyze memory separately.** Include auxiliary data structures and recursion space.
8. **Ignore constant factors for asymptotic classification.** Focus on growth as \(n\) becomes large.
9. **Check assumptions.** Average-case results depend on the input distribution, and complexity depends on the computational model.
10. **Validate practical performance.** Benchmark implementations when real execution time matters.

### 4.11 Asymptotic analysis versus empirical analysis

| Asymptotic analysis | Empirical analysis |
|---|---|
| Mathematical analysis of growth. | Measurement through experiments. |
| Often independent of specific hardware. | Depends on hardware, software, compiler, and environment. |
| Helps predict behavior for large inputs. | Shows actual performance for tested inputs. |
| Can be performed before implementation. | Requires an implementation to run. |
| Example: deriving \(\Theta(n^2)\) from nested loops. | Example: measuring execution time for arrays of different sizes. |

Both approaches are useful. Mathematical analysis explains scalability, while experiments reveal practical effects such as caching, memory allocation, and implementation overhead.

---

## 5. Worked Examples

### Example 1: Analyze a single loop

```text
FOR i ← 1 TO n
    SUM ← SUM + i
```

The loop executes \(n\) times, and each iteration performs constant work.

\[
T(n)=an+b
\]

Therefore, the time complexity is \(\Theta(n)\).

### Example 2: Analyze nested loops

```text
FOR i ← 1 TO n
    FOR j ← 1 TO n
        COUNT ← COUNT + 1
```

The inner statement executes \(n^2\) times.

\[
T(n)=an^2+bn+c
\]

Therefore, the time complexity is \(\Theta(n^2)\).

### Example 3: Analyze a halving loop

```text
i ← n
WHILE i > 1
    i ← i / 2
```

After \(k\) iterations, the value is approximately

\[
\frac{n}{2^k}.
\]

The loop stops when the value reaches 1 or less:

\[
\frac{n}{2^k}\le 1.
\]

Thus,

\[
k\ge \log_2 n.
\]

The time complexity is \(\Theta(\log n)\).

### Example 4: Compare two algorithms

Suppose Algorithm A has time complexity \(\Theta(n^2)\), while Algorithm B has time complexity \(\Theta(n\log n)\).

For sufficiently large input sizes, Algorithm B generally scales better, assuming comparable implementation and operation costs.

However, for small inputs, constant factors and implementation details may make Algorithm A competitive or even faster.

---

## 6. Quick Revision Notes

### Introduction to algorithms

- An algorithm is a finite, precise sequence of steps for solving a problem.
- Key properties include input, output, definiteness, finiteness, effectiveness, and correctness.
- Algorithms can be expressed using natural language, pseudocode, flowcharts, or code.

### Role of algorithms in computing

- Algorithms enable problem-solving, automation, and data processing.
- They are used in searching, sorting, security, AI, navigation, databases, and operating systems.
- Correctness and efficiency are both necessary.

### Nature and size of input

- Input size measures the amount of data.
- Input nature describes properties such as order, values, distribution, and structure.
- Best, average, and worst cases describe different resource requirements for inputs of the same size.

### Principles of algorithm analysis

- Time complexity measures growth in computational work.
- Space complexity measures growth in memory usage.
- \(O\) gives an asymptotic upper bound, \(\Omega\) a lower bound, and \(\Theta\) a tight bound.
- Common growth rates include constant, logarithmic, linear, linearithmic, quadratic, and exponential.

---

## 7. Important Exam Questions

### Q1. Define an algorithm and explain its characteristics.

An algorithm is a finite sequence of well-defined instructions for solving a problem. Its characteristics include input, output, definiteness, finiteness, effectiveness, and correctness.

### Q2. Explain the role of algorithms in computing.

Algorithms provide systematic solutions to computational problems, support automation, improve resource efficiency, and enable tasks such as searching, sorting, encryption, navigation, and data processing.

### Q3. What is input size? Why is it important?

Input size measures the amount of data processed by an algorithm. It is important because time and memory requirements are analyzed as functions of input size.

### Q4. Differentiate between input size and input nature.

Input size measures how much data is supplied, whereas input nature describes its properties, such as ordering, distribution, values, and structure.

### Q5. Explain best-case, average-case, and worst-case analysis.

Best case measures minimum resource usage, average case measures expected usage under a specified distribution, and worst case measures maximum usage for a given input size.

### Q6. Differentiate between time and space complexity.

Time complexity describes how computational work grows with input size. Space complexity describes how memory requirements grow with input size.

### Q7. Explain Big O, Big Omega, and Big Theta.

Big O represents an asymptotic upper bound, Big Omega represents a lower bound, and Big Theta represents a tight asymptotic bound.

### Q8. What is asymptotic analysis?

Asymptotic analysis studies how an algorithm's resource requirements grow as input size becomes large, ignoring constant factors and lower-order terms.

### Q9. How do you analyze a nested loop?

Determine how many times each loop executes, multiply iteration counts for nested loops, and add the costs of consecutive sections. For two loops each running \(n\) times, the complexity is \(\Theta(n^2)\).

---

## Conclusion

An algorithm provides a logical solution to a problem. Input size and input nature influence the amount of work it performs, and algorithm analysis helps determine how efficiently that solution uses time and memory as the problem grows.

The central ideas to remember are:

1. Design algorithms that are correct and unambiguous.
2. Identify the size and characteristics of the input.
3. Analyze time and auxiliary space.
4. Use asymptotic notation to compare growth rates.
5. Consider both theoretical efficiency and practical performance.
