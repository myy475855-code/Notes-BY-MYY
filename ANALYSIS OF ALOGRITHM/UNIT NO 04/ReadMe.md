# Recursion and Recurrence Relations

**Course:** Analysis of Algorithms

---

## What is Recursion?

Recursion is a programming technique in which a function calls itself to solve a problem by breaking it into smaller subproblems until a base condition is reached.

For example, if we call a function `fun(4)` and it contains a recursive call `fun(n-1)`, the function will first execute `fun(4)`, then call `fun(3)`, followed by `fun(2)`, and so on until the base case is reached.

## Components of Recursion

A recursive function consists of two essential components:

| Component | Description |
|---|---|
| **Base Case** | A condition that stops further recursive calls. |
| **Recursive Case** | A condition in which the function calls itself with a smaller input. |

---

## What is a Recurrence Relation?

A recurrence relation is a mathematical equation that expresses the running time of a recursive algorithm in terms of the running time of smaller input sizes.

```
T(n) = T(n-1) + 1
```

- `T(n)` represents the running time for input size `n`.
- `T(n-1)` represents the running time for input size `n-1`.
- `1` represents the constant work performed in the current call.

In asymptotic form:

```
T(n) = T(n-1) + Θ(1)
```

---

## Recurrence Relation Using Iteration Method

```c
void A(int n)
{
    if (n > 0)
    {
        for (int i = 0; i < n; i++)
        {
            printf("%d", n);
        }
        A(n - 1);
    }
}
```

- **Function Definition:** `A(int n)` defines a recursive algorithm that takes an integer `n` as input.
- **Condition:** `if (n > 0)` checks whether `n` is greater than zero.
- **For Loop:** `for (i = 0; i < n; i++)` executes `n` times.
- **Print Statement:** `printf("%d", n)` prints the value of `n` during each iteration.
- **Recursive Call:** `A(n-1)` calls the same function with the input decreased by 1.
- **Base Case:** When `n = 0`, the condition becomes false, and recursion stops.

### Step 1: Form the Recurrence Relation

```
T(n) = T(n-1) + n
```

**Base Case:**

```
T(0) = c
```

### Step 2: First Iteration — Replace T(n−1)

Given:

```
T(n) = T(n-1) + n
```

Since:

```
T(n-1) = T(n-2) + (n-1)
```

Substituting:

```
T(n) = T(n-2) + (n-1) + n
```

### Step 3: Second Iteration — Replace T(n−2)

Since:

```
T(n-2) = T(n-3) + (n-2)
```

Substituting:

```
T(n) = T(n-3) + (n-2) + (n-1) + n
```

### Step 4: Third Iteration — Replace T(n−3)

Since:

```
T(n-3) = T(n-4) + (n-3)
```

Substituting:

```
T(n) = T(n-4) + (n-3) + (n-2) + (n-1) + n
```

### Step 5: Continue Until the Base Case

Repeating the substitution process until `T(0)`:

```
T(n) = T(0) + 1 + 2 + 3 + ... + n
```

Since `T(0) = c`:

```
T(n) = c + 1 + 2 + 3 + ... + n
```

---

## Question: Find the Recurrence Relation

```cpp
void fun(int n)
{
    if (n > 0)
    {
        for (int i = 0; i < n; i++)
        {
            for (int j = 0; j < n; j++)
            {
                cout << i + j;
            }
        }
        fun(n - 1);
    }
}
```

### Step 1: Calculate the Nested Loops Complexity

The outer loop executes `n` times, and the inner loop also executes `n` times for each iteration of the outer loop.

Therefore:

```
n × n = n²
f(n) = n²
```

### Step 2: Identify the Recursive Call

The function calls itself with a smaller input:

```
fun(n - 1);
```

Therefore, the time required for the recursive call is:

```
T(n-1)
```

### Step 3: Form the Recurrence Relation

The total running time is the sum of recursive and non-recursive work.

```
T(n) = T(n-1) + n²
```

**Base Case:**

```
T(0) = c
```

Here, `c` represents constant time.

### Expand the Recurrence

For simplicity, represent the non-recursive work as `n²`.

Given:

```
T(n) = T(n-1) + n²
```

| Step | Substitution | Result |
|---|---|---|
| 1 | Given recurrence | `T(n) = T(n-1) + n²` |
| 2 | Replace `T(n-1)` | `T(n) = T(n-2) + (n-1)² + n²` |
| 3 | Replace `T(n-2)` | `T(n) = T(n-3) + (n-2)² + (n-1)² + n²` |
| 4 | Continue to the base case | `T(n) = T(0) + 1² + 2² + ... + n²` |

---

## Practice Questions

### Question 1

Analyze the following recursive algorithm. Form its recurrence relation and solve it using the Iteration Method.

```cpp
void B(int n)
{
    if (n > 0)
    {
        for (int i = 0; i < n; i++)
        {
            cout << i;
        }
        B(n - 1);
    }
}
```

### Question 2

Analyze the following algorithm. Write its recurrence relation and solve it using the Iteration Method.

```cpp
void display(int n)
{
    if (n > 0)
    {
        for (int i = 0; i < n; i++)
        {
            cout << i;
        }
        display(n - 1);
    }
}
```

### Question 3

Create a recurrence relation for the following algorithm and solve it using the Iteration Method.

```cpp
void calculate(int n)
{
    if (n > 0)
    {
        for (int i = 0; i < n; i++)
        {
            for (int j = 0; j < n; j++)
            {
                cout << i + j;
            }
        }
        calculate(n - 1);
    }
}
```

### Question 4

What is a recurrence relation? Write the recurrence relation for the following recursive function by identifying the recursive and non-recursive work.

```cpp
void calculate(int n)
{
    if (n > 0)
    {
        for (int i = 0; i < n; i++)
        {
            for (int j = 0; j < n; j++)
            {
                cout << i * j;
            }
        }
        calculate(n - 1);
    }
}
```
