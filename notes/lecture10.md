## Chain Matrix Multiplication

### Matrix Multiplication
- Is associative.
- With $n \geq 3$ matrices, the order of multiplication leads to a different total cost.

### Chain Matrix Multiplication
- Given an array $S$ of integer pairs such that the matrix $i$ is of size $S[i].h \times S[i].w$, calculate the optimal cost of chain matrix multiplication of all $n$ matrices.

```py
# I also don't know why this algorithm works.

def OptimalMatrixMul(S):

    A = [n][n]

    for i in range(0, n):
        A[i][i] = 0

    for l in range(1, n):
        for i in range(0, n - l + 1):
            j = i + l - 1
            A[i, j] = INTEGER_MAX
            for k in range(i, j):
                t = A[i, k] + A[k+1, j]
                t += S[i].h * S[k].w * S[j].w
                if t < A[i, j]:
                    A[i, j] = t

    return A[0, n-1]
```

```py
# alternative algorithm using memoization

memo = [n][n]

def InitMemo():
    for i in range(0, n):
        for j in range(i, n):
            memo[i, j] = INTEGER_MAX

def OptMul(S, i, j):
    if memo[i, j] != INTEGER_MAX:
        return memo[i, j]

    if i == j:
        return 0

    else:
        ret = INTEGER_MAX
        for k in range(i, j):
            t = OptMul(S, i, k) + OptMul(S, k+1, j)
            t += S[i].h * S[k].w * S[j].w
            if t < ret:
                ret = t

    memo[i, j] = ret
    return ret

# main function
def OptimalMatrixMul(S[0..n-1]):
    InitMemo()
    return OptMul(S, 0, n)
```