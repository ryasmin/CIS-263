# Algorithm Analysis and Math Review

This Markdown note is a cleaned and structured version of the handwritten lecture notes. It focuses on time complexity, asymptotic notation, common loop patterns, best/worst/average case analysis, and recurrence relations.

---

## 1. What Is Time Complexity?

**Time complexity** measures how the running time of an algorithm grows as the input size `n` grows.

We usually do **not** care about exact seconds. Instead, we care about the growth pattern:

- Does the algorithm grow slowly like `log n`?
- Does it grow linearly like `n`?
- Does it grow quadratically like `n^2`?
- Does it grow exponentially like `2^n`?

When analyzing algorithms, constants are ignored. For example:

```text
O(3n) = O(n)
O(200n) = O(n)
O(n + 100) = O(n)
```

The reason is that for very large `n`, the growth rate matters more than the exact constant.

---

## 2. Basic Loop Complexity Examples

### Example 1: Constant-Time Loop

```python
# This loop runs only a fixed number of times.
for i in range(1, 4):
    print(i)
```

This loop always runs 3 times, regardless of input size `n`.

```text
Time Complexity: O(1)
```

---

### Example 2: Linear Loop

```python
n = 10

for i in range(n):
    print(i)
```

The loop runs `n` times.

```text
Time Complexity: O(n)
```

---

### Example 3: Linear Loop with Constant Step

```python
n = 1000

for i in range(0, n, 200):
    print(i)
```

The loop increases by 200 each time. It runs about `n / 200` times.

```text
O(n / 200) = O(n)
```

So the complexity is still:

```text
Time Complexity: O(n)
```

---

## 3. Nested Loop Complexity

### Example 4: Simple Nested Loop

```python
n = 5

for i in range(n):
    for j in range(n):
        print(i, j)
```

The outer loop runs `n` times. For each outer iteration, the inner loop also runs `n` times.

```text
Total operations = n * n = n^2
```

```text
Time Complexity: O(n^2)
```

---

### Example 5: Triangular Nested Loop

```python
n = 5

for i in range(n):
    for j in range(i):
        print(i, j)
```

Here, the inner loop depends on `i`.

The total number of operations is:

```text
0 + 1 + 2 + 3 + ... + (n - 1)
```

Using the summation formula:

```text
1 + 2 + 3 + ... + n = n(n + 1) / 2
```

So the complexity becomes:

```text
O(n(n + 1) / 2) = O(n^2)
```

```text
Time Complexity: O(n^2)
```

Even though the inner loop does not always run `n` times, the final growth is still quadratic.

---

## 4. Loop with Square Root Complexity

### Example 6: Increasing by `i`

```python
n = 100
p = 0

for i in range(1, n + 1):
    p = p + i
    if p > n:
        break
```

Here, the value of `p` grows as:

```text
1 + 2 + 3 + ... + k
```

Using the summation formula:

```text
k(k + 1) / 2 > n
```

Ignoring constants:

```text
k^2 > n
k > sqrt(n)
```

So the loop runs approximately `sqrt(n)` times.

```text
Time Complexity: O(sqrt(n))
```

---

## 5. Logarithmic Complexity

### Example 7: Multiplying by 2

```python
n = 100

i = 1
while i < n:
    print(i)
    i = i * 2
```

The value of `i` changes like this:

```text
1, 2, 4, 8, 16, 32, ...
```

After `k` steps:

```text
i = 2^k
```

The loop stops when:

```text
2^k >= n
```

Taking log base 2:

```text
k = log2(n)
```

```text
Time Complexity: O(log n)
```

---

### Python Demonstration

```python
import math

for n in [8, 10, 16, 100, 1000]:
    steps = math.ceil(math.log2(n))
    print(f"n = {n}, approximate steps = {steps}")
```

Expected idea:

```text
n = 8      -> about 3 steps
n = 10     -> about 4 steps
n = 16     -> about 4 steps
n = 100    -> about 7 steps
n = 1000   -> about 10 steps
```

---

## 6. Loop Dividing by 2

```python
n = 100

i = n
while i > 1:
    print(i)
    i = i // 2
```

The value of `i` changes like this:

```text
n, n/2, n/4, n/8, ...
```

This also gives:

```text
Time Complexity: O(log n)
```

---

## 7. Consecutive Loops

```python
n = 10

for i in range(n):
    print(i)

for j in range(n):
    print(j)
```

The first loop is `O(n)`. The second loop is also `O(n)`.

```text
O(n) + O(n) = O(2n) = O(n)
```

```text
Time Complexity: O(n)
```

---

## 8. Common Time Complexity Classes

From slowest growth to fastest growth:

```text
O(1) < O(log n) < O(sqrt(n)) < O(n) < O(n log n) < O(n^2) < O(n^3) < O(2^n) < O(3^n) < O(n!)
```

### Table of Common Complexities

| Complexity | Name | Example |
|---|---|---|
| `O(1)` | Constant | Accessing an array element |
| `O(log n)` | Logarithmic | Binary search |
| `O(sqrt(n))` | Square-root | Loop stopping after cumulative sum passes `n` |
| `O(n)` | Linear | Single loop over an array |
| `O(n log n)` | Linearithmic | Efficient sorting such as merge sort |
| `O(n^2)` | Quadratic | Nested loop over all pairs |
| `O(n^3)` | Cubic | Triple nested loop |
| `O(2^n)` | Exponential | Some recursive branching algorithms |
| `O(n!)` | Factorial | Brute-force permutation checking |

---

## 9. Polynomial vs Exponential Growth

For small values of `n`, exponential functions may not look very large. However, for large values of `n`, exponential functions grow much faster than polynomial functions.

Example:

```python
for n in [2, 4, 8, 16, 32]:
    print(f"n={n}, n^2={n**2}, 2^n={2**n}")
```

Output idea:

```text
n=2,  n^2=4,     2^n=4
n=4,  n^2=16,    2^n=16
n=8,  n^2=64,    2^n=256
n=16, n^2=256,   2^n=65536
n=32, n^2=1024,  2^n=4294967296
```

For large `n`, polynomial algorithms are usually much better than exponential algorithms.

---

## 10. Asymptotic Notation

Asymptotic notation describes how functions behave as `n` becomes very large.

The three major notations are:

| Notation | Meaning | Informal Description |
|---|---|---|
| Big-O | Upper bound | The function grows no faster than this bound |
| Big-Omega | Lower bound | The function grows at least this fast |
| Big-Theta | Tight bound | The function grows exactly at this rate up to constants |

---

## 11. Big-O Notation

Big-O gives an **upper bound**.

We write:

```text
f(n) = O(g(n))
```

if there exist positive constants `c` and `n0` such that:

```text
f(n) <= c * g(n), for all n >= n0
```

### Example

```text
f(n) = 2n + 3
```

We can show that:

```text
2n + 3 <= 5n, for n >= 1
```

So:

```text
f(n) = O(n)
```

This means `2n + 3` is linear.

---

## 12. Big-Omega Notation

Big-Omega gives a **lower bound**.

We write:

```text
f(n) = Omega(g(n))
```

if there exist positive constants `c` and `n0` such that:

```text
f(n) >= c * g(n), for all n >= n0
```

### Example

```text
f(n) = 2n + 3
```

Since:

```text
2n + 3 >= 2n
```

we can say:

```text
f(n) = Omega(n)
```

So `2n + 3` has a linear lower bound.

---

## 13. Big-Theta Notation

Big-Theta gives a **tight bound**.

We write:

```text
f(n) = Theta(g(n))
```

if there exist positive constants `c1`, `c2`, and `n0` such that:

```text
c1 * g(n) <= f(n) <= c2 * g(n), for all n >= n0
```

### Example

```text
f(n) = 2n + 3
```

For large enough `n`:

```text
2n <= 2n + 3 <= 5n
```

So:

```text
f(n) = Theta(n)
```

This means `2n + 3` grows linearly with a tight bound.

---

## 14. Important Clarification

Big-O, Big-Omega, and Big-Theta are **not the same as** best case, worst case, and average case.

They are mathematical bounds on functions.

Best case, worst case, and average case describe algorithm behavior under different input situations.

---

## 15. More Function Examples

### Example 1

```text
f(n) = 2n^2 + 3n + 4
```

The highest-order term is `n^2`.

So:

```text
f(n) = O(n^2)
f(n) = Omega(n^2)
f(n) = Theta(n^2)
```

---

### Example 2

```text
f(n) = n log n + n
```

The dominant term is `n log n`.

So:

```text
f(n) = Theta(n log n)
```

---

## 16. When Upper and Lower Bounds Do Not Match

Sometimes a function behaves differently depending on the input.

Example:

```text
f(n) = n^2, when n is even
f(n) = n,   when n is odd
```

In this case:

```text
Upper bound: O(n^2)
Lower bound: Omega(n)
```

There is no single tight `Theta` bound because the function behaves differently for different input values.

---

## 17. Best Case, Worst Case, and Average Case

Consider linear search.

```python
def linear_search(arr, target):
    for i in range(len(arr)):
        if arr[i] == target:
            return i
    return -1
```

Suppose the array is:

```python
arr = [10, 20, 30, 40, 50, 60, 70, 80]
```

### Best Case

The target is the first element.

```text
Number of searches = 1
Time Complexity = O(1)
```

### Worst Case

The target is the last element or not present.

```text
Number of searches = n
Time Complexity = O(n)
```

### Average Case

On average, the target may be found around the middle of the array.

Total number of searches over all possible positions:

```text
1 + 2 + 3 + ... + n = n(n + 1) / 2
```

Average number of searches:

```text
[n(n + 1) / 2] / n = (n + 1) / 2
```

So:

```text
Average Case Time Complexity = O(n)
```

---

## 18. Recurrence Relations

A recurrence relation is useful when analyzing recursive algorithms.

It expresses the running time of a problem of size `n` using the running time of smaller subproblems.

---

## 19. Recurrence Example 1: Simple Recursive Call

```python
def test(n):
    if n > 0:
        print(n)
        test(n - 1)
```

The recurrence relation is:

```text
T(n) = 1 + T(n - 1)
```

Expanding:

```text
T(n) = 1 + T(n - 1)
     = 1 + 1 + T(n - 2)
     = 2 + T(n - 2)
     = 3 + T(n - 3)
     ...
     = n + T(0)
```

So:

```text
Time Complexity: O(n)
```

---

## 20. Recurrence Example 2: Recursive Call with Loop

```python
def test(n):
    if n > 0:
        for i in range(n):
            print(i)
        test(n - 1)
```

The loop takes `n` time, and then the function calls itself with `n - 1`.

```text
T(n) = n + T(n - 1)
```

Expanding:

```text
T(n) = n + T(n - 1)
     = n + (n - 1) + T(n - 2)
     = n + (n - 1) + (n - 2) + T(n - 3)
     ...
     = n + (n - 1) + (n - 2) + ... + 1
```

Using the summation formula:

```text
1 + 2 + 3 + ... + n = n(n + 1) / 2
```

So:

```text
Time Complexity: O(n^2)
```

---

## 21. Recurrence Example 3: Recursive Call with Logarithmic Loop

```python
def test(n):
    if n > 0:
        i = 1
        while i < n:
            print(i)
            i = i * 2
        test(n - 1)
```

The loop takes `log n` time. Then the function calls itself with `n - 1`.

```text
T(n) = log n + T(n - 1)
```

Expanding:

```text
T(n) = log n + log(n - 1) + log(n - 2) + ... + log(1)
```

Using logarithm rules:

```text
log n + log(n - 1) + ... + log(1)
= log(n!)
```

So:

```text
Time Complexity: O(log(n!))
```

Using approximation, this is commonly related to:

```text
O(n log n)
```

---

## 22. Recurrence Example 4: Two Recursive Calls

```python
def test(n):
    if n > 0:
        print(n)
        test(n - 1)
        test(n - 1)
```

There are two recursive calls, each of size `n - 1`.

```text
T(n) = 2T(n - 1) + 1
```

Expanding:

```text
T(n) = 2T(n - 1)
     = 2^2T(n - 2)
     = 2^3T(n - 3)
     ...
     = 2^nT(0)
```

So:

```text
Time Complexity: O(2^n)
```

This is exponential.

---

## 23. Recurrence Example 5: Dividing by 2

```python
def test(n):
    if n > 1:
        print(n)
        test(n // 2)
```

The recurrence relation is:

```text
T(n) = 1 + T(n / 2)
```

Expanding:

```text
T(n) = 1 + T(n / 2)
     = 2 + T(n / 4)
     = 3 + T(n / 8)
     ...
```

After `k` steps:

```text
n / 2^k = 1
```

So:

```text
n = 2^k
k = log2(n)
```

Therefore:

```text
Time Complexity: O(log n)
```

---

## 24. Summary Cheat Sheet

| Code Pattern | Complexity |
|---|---|
| Single statement | `O(1)` |
| Single loop from `1` to `n` | `O(n)` |
| Loop with `i += constant` | `O(n)` |
| Loop with `i *= 2` | `O(log n)` |
| Loop with `i /= 2` | `O(log n)` |
| Two nested loops each up to `n` | `O(n^2)` |
| Three nested loops each up to `n` | `O(n^3)` |
| Loop that stops when cumulative sum exceeds `n` | `O(sqrt(n))` |
| Recursive `T(n) = T(n - 1) + 1` | `O(n)` |
| Recursive `T(n) = T(n - 1) + n` | `O(n^2)` |
| Recursive `T(n) = T(n / 2) + 1` | `O(log n)` |
| Recursive `T(n) = 2T(n - 1) + 1` | `O(2^n)` |

---

## 25. Practice Problems

### Problem 1

Find the time complexity:

```python
for i in range(n):
    print(i)
```

Answer:

```text
O(n)
```

---

### Problem 2

Find the time complexity:

```python
i = 1
while i < n:
    print(i)
    i = i * 2
```

Answer:

```text
O(log n)
```

---

### Problem 3

Find the time complexity:

```python
for i in range(n):
    for j in range(i):
        print(i, j)
```

Answer:

```text
O(n^2)
```

---

### Problem 4

Find the time complexity:

```python
def test(n):
    if n > 0:
        test(n - 1)
        test(n - 1)
```

Answer:

```text
O(2^n)
```

---

## 26. Key Takeaways

Time complexity describes how an algorithm grows with input size. Constants are ignored because we focus on large input sizes. Single loops usually produce linear complexity. Nested loops often produce polynomial complexity. Loops that multiply or divide the index by 2 usually produce logarithmic complexity. Recurrence relations help analyze recursive algorithms. Big-O is an upper bound, Big-Omega is a lower bound, and Big-Theta is a tight bound.

