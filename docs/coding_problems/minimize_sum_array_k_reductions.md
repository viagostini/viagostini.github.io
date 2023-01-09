# Minimize Sum of Array with at most K reductions

Given an array of integers `arr[]` consisting of **N** integers, the task is to minimize the sum of the given array by performing at most **K** operations, where each operation involves reducing an array element $arr[i]$ to $\left\lfloor{\frac{arr[i]}{2}}\right\rfloor$**.**

## Solution

To minimize the final sum of the array, we should try to maximize the reductions. We can do that by choosing to reduce the largest element at every iteration.

## Approach: Use Max-Heap

Python has a `heapq` module that contains functions that operate on lists with heap semantics. The only caveat here is that Python only implements a MinHeap by default, so we have a few options to circumvent that:

=== "Negate the values in the array"
    ```python
    from heapq import heapify, heappop, heappush

    def solve(a, k):
        # trick to turn min-heap into max-heap
        a = [-x for x in a]
        heapify(a)
        
        for _ in range(k):
            max_elem = -heappop(a)
            new_elem = max_elem // 2
            heappush(a, -new_elem)

        # remember to invert the signal again
        return sum((-x for x in a))
    ```
=== "Custom MaxHeap class based on `heapq`"
    ```python
    from __future__ import annotations

    import heapq

    class MaxHeap:
        """Uses negative trick to turn min-heap into max-heap"""

        def __init__(self, data: list[int] | None):
            self.data: list[int] = []

            if data is not None:
                for item in data:
                    self.push(item)

        def __getitem__(self, key) -> int:
            return -self.data[key]

        def push(self, item: int):
            heapq.heappush(self.data, -item)

        def pop(self) -> int:
            return -heapq.heappop(self.data)

    def solve(a, k):
        h = MaxHeap(data=a)

        for _ in range(k):
            max_elem = h.pop()
            new_elem = max_elem // 2
            h.push(new_elem)

        return sum(h)
    ```
## Complexity Analysis
=== "Time"
    Inserting a new item in a heap take $O(\log n)$ time where $n$ is the number of elements. Popping the largest element is $O(1)$.

    So, building our heap from the input data takes $O(n \log n)$ time and the subsequent $K$ operations will take $O(k \log n)$.
    Adding that up we get $O(n \log n + k \log n)$.

=== "Space"
    We start with a list of $N$ elements and the reductions don't change the amount of items stored in the list, so $O(n)$.