## Optimal BST

### Binary Search Tree
- Each node contains a value (key).
- All nodes to the left are smaller than the node value.
- All nodes to the right are greater than the node value.
- In an array representation, nodes are ordered from left to right.
- There is an array $d[i]$ containing the depth of each node $i$.

### Optimal BST
- Given an array $f[i]$ containing the amount of times that node $i$ will be accessed, return the minimum possible total cost of traversing every node $i$, $f[i]$ times.
- $Cost(T, f) =$ sum of all $d[i] * f[i]$

```
ex.
Values   = [10  11  12]
f        = [2   1   2]


BST #1:
           10
             \
              11
                \
                 12

index =    [1   2   3]
value =    [10  11  12]
d     =    [1   2   3]

Cost(T, f) = (2*1) + (1*2) + (2*3) = 10



BST #2:
            11
           /  \
         10    12

index =    [1   2   3]
value =    [10  11  12]
d     =    [2   1   2]

Cost(T, f) = (2*2) + (1*1) + (2*2) = 9


BST #2 is more efficient than BST #1.
```
```py
# I have no idea why this algorithm works, so if someone could explain it to me, that would be grand.

# f is an array of length n
def OptimalBST(f: list):
    
    F = [n]
    A = [n+1][n+1]
    
    F[0] = 0

    # F[i] contains the sum of f[0..i]
    for i in range(0, n):
        F[i+1] = F[i] + f[i]

    for i in range(0, n+1):
        A[i, i] = 0

    for l in range(0, n):
        for i in range(0, n + 1 - l):
            j = i + l
            A[i, j] = INTEGER_MAX
            for r in range(i, j):
                if A[i, r] + A[r+1, j] < A[i, j]:
                    A[i, j] = A[i, r] + A[r+1, j]
            A[i, j] = A[i, j] + (F[j] - F[i])
    
    return A[i, n]
```
- $\mathcal{O}(n^3)$ time complexity