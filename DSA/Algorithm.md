# Algorithms
## Big O Notation
### Definition
* **Definition**: A mathematical way to describe how **running time** or **space requirement** of an algorithm grows as the input size (often describe as $n$) increases.
* **Example 1**: An algorithm with complexity is $O(n)$ running on a sample of $N=100$ elements takes $T=0.3$ seconds, then we if running with other algorithms with different complexity, we have:

| Algorithm Complexity | Running time |
| -------- | -------- |
| $O(n)$ | $T$ |
| $O(n^2)$ | $T \times N$|
| $O(n^3)$ | $T \times N^2$|
| $O(\log n)$ | $T / N * \log_2 N$|
| $O(n \log n)$ | $T * \log_2 N$|

* **Example 2**: With above algorithm, if we change the sample size from $N$ to $N_1 = 350$, how will the running time change?

| Algorithm Complexity | Running time |
| -------- | -------- |
| $O(n)$ | $T \times \frac{N_1}{N}$ |
| $O(n^2)$ | $T \times (\frac{N_1}{N})^2$ |
| $O(n^3)$ | $T \times (\frac{N_1}{N})^3$ |
| $O(\log n)$ | $T \times \frac{\log_2 N_1}{\log_2 N}$|
| $O(n \log n)$ | $T \times \frac{N_1 \times \log_2 N_1}{N \times \log_2 N}$ |
* **Note: Be careful with $O(\log n)$ case, the increasing factor is $\frac{\log_2 N_1}{\log_2 N}$, not $\log(\frac{N_1}{N})$!**
### How to determine time complexity of code?
Depend on the code and the given programming languague (this is important), you can determine the complexity with below table:

| Notation | Pattern |
| -------- | -------- |
| $O(1)$ | A single simple operation |
| $O(\log n)$ | A operation that the input size is halved to half or with a constant $1/k$ after each step|
| $O(n)$ | A single loop from 1 to $n$ |
| $O(n^2)$ | A loop in another loop (nested loop) |
| $O(n^3)$ | Triple nested loop |
| $O(n \log n)$ | Divide and conquer operation (such as Merge Sort, Quick Sort) |
| $O(2^n)$ (or $O(m^n)$) | Recursion (with generic recursion, the complexity is $O(\text{branch}^\text{depth})$)|
| $O(n!)$ | Find all permutation |

For the detailed worst and best time and space complexity of various well known algorithm, we will present it in other separate file.

### Note on determining time complexity
* If apply for loop for two separate array $A$ and $B$, the time complexity should be $O(A + B)$, not $O(n)$.
* With programming language whose strings are immutable (Java, Python, C#), the cost of a single string concatenate ```s += char``` is $O(n)$, not $O(1)$. Therefore, the below code complexity is $O(n^2)$:
```python
s = ""
for chr in another_str:
    s += chr
```

## Common operations
### Search Algorithms
### Sort Algorithms
## Common Problem-Solving Patterns
### Multiple Pointers
### Sliding Window
### Recursion
### Dynamic Programming