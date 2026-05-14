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
Searching is an basic operation. Most common search algorithms are *Linear Search* and *Binary Search*.
#### Linear Search
Pretty simple, just iterate the list until the desired elements are found. Therefore the complexity is $O(n)$.
```python
x = 16
found = -1
for idx, element in enumerate(array):
    if element == x:
        found = idx
        break
if found != -1:
    print(f"Found x at index {found}")
else:
    print("Not found")
```

However, is this algorithm obsolete? No! It has been used in many cases and to decide when to use Linear Search, we have to consider these questions:
* Size of the array. If its size is small (i.e $N<64$ bytes, for JDK, $N < 47$, for GNU C++, $N < 16$), linear search is prefered because of both time in practice and *cache locality*.
* For the data that cannot or costly to sort (for e.g. linked list, streaming data,...), choose Linear search instead of binary search.
#### Binary Search
This algorithm is only applicable for the sorted array. It divide the data in half for each iteration until the desired element is found. Therefore, its complexity is $O(\log n)$:
```python
def binary_seach(array, x):
    # array = sorted(array)
    low, high = 0, len(array) - 1
    while low <= high:
        mid = low + (high - low) // 2  # Preventing overflow
        if array[mid] == x:
            return mid
        elif array[mid] > x:
            high = mid - 1
        else:
            low = mid + 1
    return -1
```

Even though the binary search is considered faster, compared to Linear search, there are some disadvantages such: 
* **Cache locality**: For the small size array, the cost of accessing element in RAM can make it slower than linear search (where reading elements in cache is faster)
* **Branch Misprediction**: All modern CPU has a stuff called "Branch Predictor", and many sources claim that the ```if``` condition with probability $50/50$ sucks! Therefore, modern C++ library, all ```if``` are replaced with bitwise operations or ```cmov``` (conditional move).

For the extentions of binary search, it can be used to find both upper bound (the minimum element $x$ that satisfies $x > \text{target}$) or lower bound (the minimum element $x$ that satisfies $x \ge \text{target}$).

```python
def lower_bound_seach(array, x): # (upper)
    # array = sorted(array)
    low, high = 0, len(array)          # Instead of len(array) - 1
    while low < high:                  # Instead of low <= high
        mid = low + (high - low) // 2  # Preventing overflow
        if array[mid] >= x:            # If we need to find upper bound then array[mid] > x
            high = mid                 # mid maybe the return value, so high = mid instead of high = mid - 1
        else:
            low = mid + 1
    return low
```
Some advanced optimizations of Binary Search are **Exponential Search** and **Interpolation Search**, below is their ideas:
* **Exponential Search**: Used when the size is unknown, we find the initial range for binary search by considering exponential indexes $1, 2, 4, 8,...$ until the upper bound is found.
* **Interpolation Search**: When the target array is uniformly distributed sorted data or the cost of probing (accessing an element) is high (note, do not use this when the data distribution is unknown or skewed since it can be potentially slow as Linear search), the complexity of search can be optimized as $O(\log (\log n))$ by using this formula to find ```mid``` position:
$$pos = low + \left[ \frac{(target - arr[low]) \times (high - low)}{arr[high] - arr[low]} \right]$$

Beside above search algorithms, we can optimize the search by ultilizing a appropriate **data structure** (such as Hashmap,...). However, it is out of the scope here.
### Sort Algorithms
#### Basic Sorting Algorithms
In this section, we take a look at three algorithms: Insertion sort, Bubble sort and Selection sort. They are comparison-based since they compare pairs of elements in the array to decide whether swap them or not. They are the easiest to implement but not so efficient as their overall complexity is $O(n^2)$:
* **Bubble sort**: Repeatly iterate the whole array to swap two adjacent elements if their are in wrong place. At the end of each iteration, the largest will "bubble up" to the end.
* **Selection sort**: Scan the unsorted positions to find the minimum and swap it to the front.
* **Insertion sort**: Build a sorted section. At each time, an unsorted element is picked and insert to the right position in the sorted section.

When to use these algorithms? Even though the bad complexity, they are commonly used today in below cases:
* The array size is small.
* The array is nearly sorted (As insertion sort and bubble sort complexity are both $O(n)$ for the sorted array).
* There are tight memory constraints, as their space complexity are both $\Omega(1)$.

### Advanced Sorting Algorithms 
In this section, we take a look at three algorithms: Quick Sort, Merge Sort and Heap Sort. But before diving in, let's talk about these characteristics:
* **Stability**: Are two elements with same value swapped during the algorithm? In multi sort (sort multiple times with different criterias), the stability make sure that multiple sort can be executed without a following sort corrupting the output of previous sort.
* **In-place**: Do the algorithm need extra space for executing? When running an algorithm in a memory constrained environment, it is the best to control the algorithm's memory so it will not cause "Out of memory".

#### Quick Sort

| Algorithm | Best (Time) | Average (Time) | Worst (Time) | Space (Worst) |
| :--- | :--- | :--- | :--- | :--- |
| **Quicksort** | $\Omega(n \log(n))$ | $\theta(n \log(n))$ | $O(n^2)$ | $O(\log(n))$ |

The idea of quick sort is D&C (Note: in Divide step, we split the array into three parts, and in the Conquer step, we do nothing!). Here is the original version:
```python
def quicksort_canonical(arr):
    if len(arr) <= 1:
        return arr

    pivot = arr[len(arr) // 2]
    left_part = [x for x in arr if x < pivot]
    middle_part = [x for x in arr if x == pivot]
    right_part = [x for x in arr if x > pivot]
    return quicksort_canonical(left_part) + middle_part + quicksort_canonical(right_part)
```

The original version is neither stable nor inplace. Below is the semi inplace version:

```python
def partition(arr, low, high):
    pivot = arr[high]
    i = low - 1

    for j in range(low, high):
        if arr[j] <= pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]

    arr[i + 1], arr[high] = arr[high], arr[i + 1]
    return i + 1

def quicksort_inplace(arr, low, high):
    if low < high:
        pi = partition(arr, low, high)

        quicksort_inplace(arr, low, pi - 1)
        quicksort_inplace(arr, pi + 1, high)
```

The space complexity of this version is $O(\log n)$.

Choosing the pivot is important. Quicksort achieved the best performance when the pivot always divide the current array into two parts that has approximately same size. The worst case is the sorted or nearly sorted array, where the complexity can grow up to $O(n^2)$.

#### Merge Sort
| Algorithm | Best (Time) | Average (Time) | Worst (Time) | Space (Worst) |
| :--- | :--- | :--- | :--- | :--- |
| **Mergesort** | $\Omega(n \log(n))$ | $\theta(n \log(n))$ | $O(n \log(n))$ | $O(n)$ |

Merge sort is also a D&C algorithm. Different from Quicksort, Merge sort guarantees the complexity $O(n \log(n))$ in all cases:
```python
def merge_sort(arr):
    if len(arr) <= 1:
        return arr

    mid = len(arr) // 2
    left_half = merge_sort(arr[:mid])
    right_half = merge_sort(arr[mid:])

    return merge(left_half, right_half)

def merge(left, right):
    sorted_arr = []
    i = j = 0

    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            sorted_arr.append(left[i])
            i += 1
        else:
            sorted_arr.append(right[j])
            j += 1

    sorted_arr.extend(left[i:])
    sorted_arr.extend(right[j:])
    
    return sorted_arr
```
As the trade off for the time complexity, its space complexity is $O(n)$. So to sort the data with the size 100GB with only 8GB RAM, we need **external merge sort**.

Merge sort is behind modern ```sort()``` function in Java and Python, which use a advanced version of merge sort - Timsort. It is the hybrid sorting algorithm between merge sort and insertion sort.

| Algorithm | Best (Time) | Average (Time) | Worst (Time) | Space (Worst) |
| :--- | :--- | :--- | :--- | :--- |
| **Timsort** | $\Omega(n)$ | $\theta(n \log(n))$ | $O(n \log(n))$ | $O(n)$ |

#### Heap Sort

| Algorithm | Best (Time) | Average (Time) | Worst (Time) | Space (Worst) |
| :--- | :--- | :--- | :--- | :--- |
| **Heapsort** | $\Omega(n \log(n))$ | $\theta(n \log(n))$ | $O(n \log(n))$ | $O(1)$ |

Look at the algorithm stat, it seems like Heapsort is the best. But in reality, the running time of Heapsort is not as good as other advanced algorithms. The reason is the **cache locality** characteristic, where other algorithm can load and access elements quickly from cache, Heapsort requires reading each element from RAM. Moreover, it is unstable. Therefore Heapsort is only considered when we need an algorithm to run on embbeded system.

### Linear time Sorting Algorithms
| Algorithm | Best (Time) | Average (Time) | Worst (Time) | Space (Worst) |
| :--- | :--- | :--- | :--- | :--- |
| **Bucket Sort** | $\Omega(n+k)$ | $\theta(n+k)$ | $O(n^2)$ | $O(n)$ |
| **Radix Sort** | $\Omega(nk)$ | $\theta(nk)$ | $O(nk)$ | $O(n+k)$ |
| **Counting Sort** | $\Omega(n+k)$ | $\theta(n+k)$ | $O(n+k)$ | $O(k)$ |

When we need to sort the data that has a detailed range such as ages, scores, zip codes,..., these linear time sorting algorithms must be in your prioritized options. They are Bucket Sort, Radix Sort and Counting Sort.

<!-- ## Common Problem-Solving Patterns
### Multiple Pointers
### Sliding Window
### Recursion
### Dynamic Programming -->