## Lecture 1: Introduction

### Key definitions
- `Algorithm:` a method to compute a desired output from the given input
- `Problem:` described by the set of possible inputs and the desired output.
- `Computational Model:` the set of elementary operations used by the algorithm

### Big-O notation
- Intuition: the big-O notation lets us state the runtime/space of an algorithm, ignoring details that do not matter for large n (constant factors and lower-order terms).
- $\mathcal{O}$ = upper bound
    - Formal proof:
        - Choose $n_0 \in \mathbb{R}$ and $C \in \mathbb{R}$.
        - If for all $n \geq n_0$, $f(n) \leq C * g(n)$, then $f(n) = \mathcal{O}(g(n))$
    - $5n = \mathcal{O}(n)$
    - $5n = \mathcal{O}(n^2)$
- $\Omega$ = lower bound
    - Formal proof:
        - Choose $n_0 \in \mathbb{R}$ and $C \in \mathbb{R}$.
        - If for all $n \geq n_0$, $f(n) \geq C * g(n)$, then $f(n) = \Omega(g(n))$
    - $5n^2 = \Omega(n)$
    - $n! = \Omega(n)$
- $\Theta$ = tight bound
    - Formal proof:
        - Both conditions for $\mathcal{O}(g(n))$ and $\Omega(g(n))$ hold.
    - $5n = \Theta(n)$
    - $5n^2 = \Theta(n^2)$

### Integer exponentiation by squaring
- Input
    - integers $a, m > 0$ and n $\geq 0$
- Output
    - the value $a^n \mod m$
```py
def FastPow(a, n, m):
    if n == 0:
        return 1 % m
    if n is odd:
        t = FastPow(a, (n - 1)/2, m)
        return (t * t * a) % m
    else:
        t = FastPow(a, n/2, m)
        return (t * t) % m
```

### Bubble sort
- time complexity: $O(n^2)$

### Binary Search
```py
def BinarySearch(A, x):
    low = 1
    high = A.length - 1
    while low + 1 < high:
        mid = floor((low + high) / 2)
        if A[mid] > x:
            high = mid
        else:
            low = mid
    if A[low] = x:
        return True
    else:
        return False
```
- time complexity: $O(\log n)$
    - In each step, we split the range into half.
    - Correctness is usually the bigger problem, edge cases:
        - A.length = 0 or 1
        - search range A[low... high - 1] satisfies high - low <= 3

### Greatest Common Divisor
```py
def euclid(a, b):
    if b == 0:
        return a
    else:
        return euclid(b, a % b)
```
- time complexity: $\mathcal{O}(\log(a+b))$


### Least Common Ancestor
- Part 1
    - Traverse tree and store the entry $a$ and exit $b$ for each node.
    - $u$ is an ancestor of $v$ only if $a(u) \leq a(v) \leq b(v) \leq b(u)$.
    - $\mathcal{O}(n)$ time preprocessing
    - $\mathcal{O}(1)$ time query
- Jumping $k$ nodes up in the tree
    - $jump[v][i]$ contains the node at distance $2^i$ from node $v$ to root.
    - $\mathcal{O}(n \log n)$ space for all $v$
    - Precomputed in $\mathcal{O}(n \log n)$ time:
        - $jump[v][0] = parent(v)$
        - $jump[v][i] = jump[jump[v][i-1]][i-1]$